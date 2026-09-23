---
tags: [state/closed/2023/3]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:32:31.495+02:00
---

# T2023-0104-1458 Infomaniak Cloud Setup
#state/closed/2023/3
## Description
Website
<https://www.infomaniak.com/de/hosting/public-cloud>

Create a cloud account and some instances to test it.

## Todo

## Done
### 2023-01-04
- Created Task
### 2023-01-05
- Setup PublicCloud project and user
- Installation openstackclient
  [[OpenStack]]
- Create test vm
- Create OPNsense firewall
- Create WireGuard access

### 2023-01-12
- Delete the whole project and recreate it after issues. It was not possible to access the vms behind the firewall any more.
- Create [[OpenStack#Terraform]] and [[Ansible#Roles#tf_infomaniak]]
- Tested 1_init and 2_base playbooks on the new host ubuntu-1 in the infomaniak cloud. works fine :)
- Issue: ubuntu-1 gets wrong dns 192.168.1.1 and domain dc3-a.pub1.infomaniak.cloud
  - Solution: Set renderer networkd for netplan
  ```bash
  network:
      version: 2
      renderer: networkd
      ethernets:
  ```
Looks like this still causes issues at boot. Somehow the infomaniak settings with the wrong dns ip 192.168.1.2 and search domain (...infomaniak.cloud) still are applied first. This may also happen after `resolvctl flush-caches`
```bash
resolvectl 
Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (ens3)
    Current Scopes: DNS
         Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 192.168.1.1
       DNS Servers: 192.168.1.1 192.168.1.2
        DNS Domain: dc3-a.pub1.infomaniak.cloud localdomain
```
What seems to help is to set the global DNS in /etc/systemd/resolvd.conf:
```bash
[Resolve]
. . .
DNS=192.168.1.1
. . .
```
It looks like this now after boot:
```bash
resolvectl 
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub
Current DNS Server: 192.168.1.1
       DNS Servers: 192.168.1.1

Link 2 (ens3)
    Current Scopes: DNS
         Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 192.168.1.2
       DNS Servers: 192.168.1.2
        DNS Domain: dc3-a.pub1.infomaniak.cloud
```
And then after the updated setting from the dhcp server:
```bash
resolvectl 
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub
Current DNS Server: 192.168.1.1
       DNS Servers: 192.168.1.1

Link 2 (ens3)
Current Scopes: DNS
     Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
   DNS Servers: 192.168.1.1
    DNS Domain: localdomain
```
### 2023-01-16
There are still the same issues now.
Tried the following for netplan
- disable to set nameserver and search domain via dhcp for the specific interface
- set both manually
```bash
network:
    version: 2
    renderer: networkd
    ethernets:
        ens3:
            match:
                macaddress: fa:16:3e:a8:dd:0a
            dhcp4: true
            dhcp4-overrides:
              use-dns: false
              use-domains: false
            nameservers:
              search: [me-cloud.aa]
              addresses: [192.168.1.1]
            mtu: 1500
            set-name: ens3
```
And on OPNsense (System > Settings > General)
DNS server options: Allow but **exclude LAN**

Still the same issue, after reboot it was fine. then running dhclient again the infomaniak dns server...

Now completely unchecked (disallowed) DNS to be changed on the firewall by DHCP

Next try. There seems to be dhcp enabled on the openstack subnet. Disable it:
```bash
openstack subnet set --no-dhcp opnsense-lan-subnet
```

That should be the solution now.

Now I have the following issue. As soon as I run dhcpclient and get a new les from my own dhcp I loos connection to the client. It looks to me like the use of the openstack/infomaniak dhcp is forced...
In addition I have removed the allocation pool from the subnet.

Tried to create a new ubuntu server failed with the following error:
```bash
'Build of instance 8320bd74-1987-42d6-9b42-8126572690b1 aborted: Failed to allocate the network(s) with error No fixed IP addresses available for network: ebf0e5a3-d8fa-4456-b098-64dfa01a555a, not rescheduling.'
```

Next try. Use the openstack dhcp service with the correct dns settings etc.
```bash
(base) me@hp-me:~$ openstack subnet set --allocation-pool start=192.168.1.100,end=192.168.1.250 --dhcp --dns-nameserver 192.168.1.1 --gateway 192.168.1.1 opnsense-lan-subnet

(base) me@hp-me:~$ openstack subnet show opnsense-lan-subnet
+----------------------+--------------------------------------+
| Field                | Value                                |
+----------------------+--------------------------------------+
| allocation_pools     | 192.168.1.100-192.168.1.250          |
| cidr                 | 192.168.1.0/24                       |
| created_at           | 2023-01-11T08:40:09Z                 |
| description          |                                      |
| dns_nameservers      | 192.168.1.1                          |
| dns_publish_fixed_ip | None                                 |
| enable_dhcp          | True                                 |
| gateway_ip           | 192.168.1.1                          |
| host_routes          |                                      |
| id                   | db010d90-e5b6-4f02-8bbb-28a7efbfed2f |
| ip_version           | 4                                    |
| ipv6_address_mode    | None                                 |
| ipv6_ra_mode         | None                                 |
| name                 | opnsense-lan-subnet                  |
| network_id           | ebf0e5a3-d8fa-4456-b098-64dfa01a555a |
| project_id           | 83a42d910a36481ab43e8c966d2bf7a6     |
| revision_number      | 5                                    |
| segment_id           | None                                 |
| service_types        |                                      |
| subnetpool_id        | None                                 |
| tags                 |                                      |
| updated_at           | 2023-01-16T16:47:52Z                 |
+----------------------+--------------------------------------+

```
