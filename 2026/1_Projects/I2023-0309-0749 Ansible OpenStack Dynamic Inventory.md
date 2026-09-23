---
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:29:10.606+02:00
---

# I2023-0309-0749 Ansible OpenStack Dynamic Inventory
## Problem description
Created dynamic inventory as described here: [[I2023-0309-0749 Ansible OpenStack Dynamic Inventory]] and here [[OpenStack#Client#Config Files]]

```bash
me@hp-me:~/.ansible$ ansible-inventory -i ./openstack.yml --list -y -vv
ansible-inventory 2.10.8
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/me/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible-inventory
  python version = 3.10.6 (main, Nov 14 2022, 16:10:14) [GCC 11.3.0]
Using /etc/ansible/ansible.cfg as config file
[WARNING]:  * Failed to parse /home/me/.ansible/openstack.yml with auto plugin: Incompatible openstacksdk
library found: Version MUST be >=1.0 and <=None, but 0.61.0 is smaller than minimum version 1.0
[WARNING]:  * Failed to parse /home/me/.ansible/openstack.yml with yaml plugin: Plugin configuration YAML file,
not YAML inventory
[WARNING]:  * Failed to parse /home/me/.ansible/openstack.yml with ini plugin: Invalid host pattern 'plugin:'
supplied, ending in ':' is not allowed, this character is reserved to provide a port.
[WARNING]: Unable to parse /home/me/.ansible/openstack.yml as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
all:
  children:
    ungrouped: {}

```

## Cause

## Solution
