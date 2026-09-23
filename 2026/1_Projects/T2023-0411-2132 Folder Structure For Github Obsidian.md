---
tags: [state/closed/2023/11/10]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:34:05.650+02:00
---

# T2023-0411-2132 Folder Structure For Github Obsidian
#state/closed/2023/11/10
## Description

## Todo

## Done
### 2023-11-10
Documentation in [[Obsidian]]
### 2023-04-11
- Created Task

Folders
```
find . -maxdepth 3 -type d | grep -vE "trash|obsidian"
```
Output
```
.
./3_Resources
./2_Areas
./2_Areas/Scripts
./2_Areas/Scripts/Attachments
./2_Areas/Systems
./2_Areas/Attachments
./Templates
./4_Archive
./1_Projects
./1_Projects/NewIssues
./1_Projects/Home
./1_Projects/Home/Attachments
./1_Projects/Home/Issues
./1_Projects/Home/Tasks
./1_Projects/Intersim
./1_Projects/Intersim/Issues
./1_Projects/Intersim/Tasks
./1_Projects/NewTasks
```

The three git repositories are
obsidian_vault
```
.
./3_Resources
./2_Areas
./2_Areas/Scripts
./2_Areas/Scripts/Attachments
./2_Areas/Systems
./2_Areas/Attachments
./Templates
./4_Archive
./1_Projects
./1_Projects/NewIssues
./1_Projects/NewTasks
```
home_vault
```
./1_Projects/Home
./1_Projects/Home/Attachments
./1_Projects/Home/Issues
./1_Projects/Home/Tasks
```
intersim_vault
```
./1_Projects/Intersim
./1_Projects/Intersim/Issues
./1_Projects/Intersim/Tasks
```

Create the three private repositories on github
- obsidian_vault
- home_vault
- intersim_vault

Create SSH key for each repository
```bash
 2030  ssh-keygen -t ecdsa -f obsidian_vault_id_ecdsa -N ""
 2031  ssh-keygen -t ecdsa -f home_vault_id_ecdsa -N ""
 2032  ssh-keygen -t ecdsa -f intersim_vault_id_ecdsa -N ""
```

Create ssh config to link each key to a hostname
~/.ssh/config
```bash
Host github.com-obsidian_vault
Hostname github.com
IdentityFile ~/.ssh/obsidian_vault_id_ecdsa
User git

Host github.com-home_vault
Hostname github.com
IdentityFile ~/.ssh/home_vault_id_ecdsa
User git

Host github.com-intersim_vault
Hostname github.com
IdentityFile ~/.ssh/intersim_vault_id_ecdsa
User git
```

Create the directory obsidian_vault and the subdirectories 1_Project/Home 1_Project/Intersim for the three repositories.

Then clone the github repos into each folder
```bash
cd obsidian_vault
git clone git@github.com-obsidian_vault:elchior/obsidian_vault.git
cd 1_Project
git clone git@github.com-obsidian_vault:elchior/obsidian_vault.git
git clone git@github.com-obsidian_vault:elchior/obsidian_vault.git
```
