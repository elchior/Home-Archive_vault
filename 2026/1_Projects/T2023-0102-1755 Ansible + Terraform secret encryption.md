---
tags: [state/closed/2022/12]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:32:18.785+02:00
---

# T2023-0102-1755 Ansible + Terraform secret encryption
#state/closed/2022/12
## Description
Encrypt password / secrets used by Ansible and Terraform so that it is possible to safely copy the configuration folders to a gitlab or github.

## Todo

## Done
### 2023-01-02
- Created Task

### 2022-12-30
- Use Ansible-vault to encrypt passwords and credentials (Terraform client)
  - Ansible-vault can not be used for Terraform
  - It is possible to use variables set in Ansible with the Terraform config. The solution therefore will be to set the variables in Terraform and encrypt the variables using ansible-vault.
    [[Ansible#Roles#tf_deploy]]
