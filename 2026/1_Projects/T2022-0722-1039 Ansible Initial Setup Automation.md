---
tags: [state/closed/2022/11]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:30:04.202+02:00
---

# T2022-0722-1039 Ansible Initial Setup Automation
#state/closed/2022/11
## Description
Planing the initial setup of a newly created VM on Proxmox with Ansible.

## Todo

## Conclusion
### System Setup
#### Create New Template
[[Proxmox#Howto#Create Template#Ubuntu]]
#### Manual: Clone VM
[[Ansible#Roles]]

## Done
### 2022-07-22
- Created Task
- Create initial plan to setup systems
- Create base plan
  - up to vim which still needs improvements (config file access rights)

### 2022-08-21
- Changed permision of the vimrc files in the home directories
   ==todo: check file permissions==
     They need to be set for each user...
   see <https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html> and <https://www.redhat.com/sysadmin/ansible-configure-vim>
- Added task to configure
  - bash history
  - aliases
