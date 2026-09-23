---
tags: [state/open/2]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:29:32.456+02:00
---

# T2021-1111-2034 pfSense Setup
#state/open/2

## GOAL
- Download
<https://www.pfsense.org/download/>
- Documentation
<https://docs.netgate.com/pfsense/en/latest/install/index.html>
- Plug in usb stick
- Check the device name `dmesg | tail`
- Erase disk `sudo dd if=/dev/zero of=/dev/sdb bs=1M count=1`
- Copy image to disk
  ```
  cd ~/Downloads
  sudo dd if=pfSense-CE-memstick-2.5.2-RELEASE-amd64.img of=/dev/sdb bs=4M
  ```
- Accept License
- Start install process
- Select keyboard layout (default US)
-  Partitioning: Auto (ZFS)
- Final modifications: No
- Reboot
- Assign interfaces
- Enable VLAN

## ACTION
