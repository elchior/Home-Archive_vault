---
tags: [state/closed/2023/3]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:33:52.273+02:00
---

# T2023-0317-1403 Ansible Initial User Setup Roles
#state/closed/2023/3
## Description
Create a basic user management role similar to [[Ansible Role 1_initial]]

Compared to the above role it would be better if it is possible to use an existing role that can be used on multiple Linux distributions.

What the role(s) should do:
- Add sudo
- Create sudo admin group
- Create an ansible user
- Remove ubuntu user
- Create user me

## Todo

## Done
### 2023-03-17
- Created Task
- Define role to use: <https://galaxy.ansible.com/GROG/management-user>
- Created issue, because of wrong grog.sudo version dependency:
  <https://github.com/GROG/ansible-role-management-user/issues/10>
- Created [[Ansible Role Initial User Setup]]Create Playbook that combines all roles.
### 2023-03-28
- Fix issue [[Ansible Role Initial User Setup#Issue]]
- Create Playbook that combines all roles.
