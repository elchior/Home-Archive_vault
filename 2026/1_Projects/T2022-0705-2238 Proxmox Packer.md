---
tags: [state/closed/2022/7]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:30:01.551+02:00
---

# T2022-0705-2238 Proxmox Packer
#state/closed/2022/7
## Description
Packer configuration.

## Conclusion
Packer does not seem to be the best solution for ubuntu.
- The Ubuntu Live ISOs packer has to use are big.
- It is not possible to do a minimal installation with the Live ISO

Therefore I have used a [[Cloud Image Script|script]] which downloads a ubuntu cloud image file and creates a template from this.
- The setup is much faster
- The VMs created from this are smaller
- There is a minimum cloud image which uses again about 500 MB less disk space (996M)

## Todo
## Done
### 2022-07-19
- Finished basic work on the script [[Cloud Image Script]]

### 2022-07-12
- Documentation [[Packer]]

### 2022-07-08
- added authorized_keys file for the user me using a shell provisioner from packer.
### 2022-07-05
- Created Task
