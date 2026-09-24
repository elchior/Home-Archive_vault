---
tags: [state/closed/2023/3]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:32:13.004+02:00
---

# T2022-1217-2234 Ansible Automation Part Two
#state/closed/2023/3
## Description
First do [[Improvments for cloud-image.bsh Script]]

Automate setup of new vms with Ansible
- Run the [[Cloud Image Script|cloud-image.bsh]] script to create a new template
- Create the virtual machines with [[Terraform]]
- Run the [[Ansible]] roles after the vm setup

## Todo

## Done
### 2022-12-17
- Created Task

### 2022-12-30
Automate setup of new vms with Ansible
- Run cloud-image.bsh script to create a new template
  [[Ansible#Roles#pve_template]]
- Create the virtual machines with Terraform
  [[Ansible#Roles#tf_deploy]]
- Run Ansible roles after the vm setup
  [[Ansible#Roles#1_initial]]
  [[Ansible#Roles#2_base]]
