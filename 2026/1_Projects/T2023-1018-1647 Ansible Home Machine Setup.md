---
tags: [state/closed/2023/11/20, todo/ansible/configuration]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:34:16.150+02:00
---

# T2023-1018-1647 Ansible Home Machine Setup
#state/closed/2023/11/20
## Description
Create Ansible playbooks to do a complete home setup for my workstation/notebook.

- Add repositories
- Install software
  - [[Nala#Installation]]
  - [[Ansible#Installation]]
  - [[Vim]]
  - [[tmux#Installation]]
  - [[Git#Installation]]
  - [[Spotify#Installation]]
  - [[Regolith#Installation]]
  - [[OpenStack#Install openstack client]]
  - [[WireGuard Road-Warrior (Point-to-Site)#Configure Endpoint (Client/Point) on Ubuntu#Installation]]
  - [[YubiKey#Install Software (Ubuntu)#Basic Tools]]
  - [[Shutter#Installation]]
  - [[Brave#Installation]]
  - [[AppImageLauncher#Installation]]
  - libreoffice
  - [[Terraform]]
  - [[Timeshift]]
  - [[Espanso]]
  - Additional Role for VS Code
    - ngetchell.vscode
      `ansible-galaxy install -f --roles-path ~/.ansible/roles/share/ ngetchell.vscode`
      see [[Visual Studio Code#Installation#Ansible|code]]
  - App Images
    Additinal role appimage
    `ansible-galaxy install -f --roles-path ~/.ansible/roles/share/ spreadcat.appimage`
    see <https://github.com/Spreadcat/ansible-role-appimage>
    - [[Obsidian#Installation]]
    - [[kDrive#Installation / Upgrade]]
    - [[YubiKey#Install Software (Ubuntu)#YubiKey Manager GUI]]
  - Optional
    - (Firefox)
    - (Pipwire-pulse, wireplumber)
    - (kubectl)
    - ([[Docker#Installation]])
- Create home folder structure
- Configure software
  - [x] [[Ansible]]
  - [x] [[Vim]]
  - [x] [[tmux]]
  - [x] [[Git]]
  - Gnome
  - [[Regolith]]
  - [[Screen Lock]]
  - [[OpenStack]]
  - [[Shutter]]
  - [[Brave]]
  - [x] [[Visual Studio Code|code]]
  - [x] [[Terraform]]
  - [x] [[Synology DS 218+]] / [[Samba Share#Mount Share]]

## Todo
- Ansible configurations
#todo/ansible/configuration

## Documentation
### Software installation
### Tools to install
- regolith
  GPG Key: <https://regolith-desktop.org/regolith.key>
  Repository: deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] <https://regolith-desktop.org/release-3_0-ubuntu-jammy-amd64> jammy main
- brave browser
  GPG Key: <https://brave-browser-apt-release.s3.brave.com/brave-core.asc>
  Repository: deb [arch=amd64] <https://brave-browser-apt-release.s3.brave.com/> stable main
- terraform
  GPG Key: <https://apt.releases.hashicorp.com/gpg>
  Repository: deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] <https://apt.releases.hashicorp.com> jammy main
- (docker)
  GPG Key:
  Repository:

PPA (Personal Package Archive) Repositories
- youbikey basic tools
  ppa:yubico/stable
- AppImageLauncher
  ppa:appimagelauncher-team/stable

App Image Applications
- kdrive
- Obsidian
- yubikey manager
  ==only works with the app image version -> app image install==

Manual Download and Installation
- protonvpn
  Download: <https://repo.protonvpn.com/debian/dists/stable/main/binary-all/protonvpn-stable-release_1.0.3-2_all.deb>
  Check:
  `echo "c68a0b8dad58ab75080eed7cb989e5634fc88fca051703139c025352a6ee19ad  protonvpn-stable-release_1.0.3-2_all.deb" | sha256sum --check -`
  Update and Install:
  `sudo apt-get update && sudo apt-get install proton-vpn-gnome-desktop`
  - See for details: <https://protonvpn.com/support/official-ubuntu-vpn-setup/>
- nala
  Manual steps require to download and install a .deb package which will add the repository

### Adding the missing repositories
see <https://tannguyen.dev/2020/08/setting-up-your-local-machine-using-ansible/>

New role
```yaml
cd ~/.ansible/roles
ansible-galaxy init home_machine_setup
```

Create task home_machine_setup/tasks/repo_setup.yml
```bash
- name: "Add gpg keys"
  apt_key:
    url: "{{ item }}"
    state: present
  loop:
    # Regolith Desktop
    - "https://regolith-desktop.org/regolith.key"
    # Brave Browser
    - "https://brave-browser-apt-release.s3.brave.com/brave-core.asc"
    # Hashicorp (terraform)
    - "https://apt.releases.hashicorp.com/gpg"

- name: add repositories
  apt_repository:
    repo: "{{ item.repo }}"
    filename: "{{ item.filename }}"
    state: present
    update_cache: false
  with_items:
    - { repo: "deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg]  https://regolith-desktop.org/release-3_0-ubuntu-jammy-amd64 jammy main", filename: "regolith" }
    - { repo: "deb [arch=amd64] https://brave-browser-apt-release.s3.brave.com/ stable main", filename: "brave" }
    - { repo: "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com jammy main", filename: "hashicorp" }
```

main.yml
```bash
---
# tasks file for home_machine_setup
- name: "Repo Setup"
  import_tasks: repo_setup.yml
```

Playbook home_machine_setup.yml
```bash
- name: Replace management user ubuntu with ansible
  hosts: localhost
  connection: local
  roles:
    - role: home_machine_setup
      become: yes
```

### Issues
#### 1
Problem
```bash
sudo apt update
E: Conflicting values set for option Trusted regarding source https://regolith-desktop.org/release-3_0-ubuntu-jammy-amd64/ jammy
E: The list of sources could not be read.
```
Solution
```bash
sudo mv trusted.gpg.d/regolith-linux_ubuntu_release.gpg /tmp/
sudo mv sources.list.d/regolith.list* /tmp
```

#### 2
Problem
```bash
. . .
Get:24 http://ch.archive.ubuntu.com/ubuntu jammy-backports/universe amd64 DEP-11 Metadata [17.7 kB]
Get:25 https://regolith-desktop.org/release-3_0-ubuntu-jammy-amd64 jammy/main amd64 Packages [33.1 kB]
Fetched 2’733 kB in 2s (1’484 kB/s)   
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
51 packages can be upgraded. Run 'apt list --upgradable' to see them.
W: https://regolith-desktop.org/release-3_0-ubuntu-jammy-amd64/dists/jammy/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.

```
Solution

List keys
```bash
(base) me@hp-me:~/.ansible/roles$ sudo apt-key list
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
/etc/apt/trusted.gpg
--------------------
pub   rsa4096 2020-05-07 [SC]
      E8A0 32E0 94D8 EB4E A189  D270 DA41 8C88 A321 9F7B
uid           [ unknown] HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
sub   rsa4096 2020-05-07 [E]

pub   rsa3072 2021-08-15 [SC] [expired: 2023-08-15]
      FB9C EE65 6F30 4FFC C7A7  19F9 BFE7 1764 9A5C 3D08
uid           [ expired] Regolith Linux <regolith.linux@gmail.com>

pub   rsa3072 2023-08-17 [SC]
      C91E CAB8 6203 7F94 7408  7DBC 7107 DED1 3350 5B88
uid           [ unknown] Regolith Linux <regolith.linux@gmail.com>
sub   rsa3072 2023-08-17 [E]

pub   rsa4096 2023-01-10 [SC] [expires: 2028-01-09]
      798A EC65 4E5C 1542 8C8E  42EE AA16 FCBC A621 E701
uid           [ unknown] HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
sub   rsa4096 2023-01-10 [S] [expires: 2028-01-09]

/etc/apt/trusted.gpg.d/brave-browser-release.gpg
------------------------------------------------
pub   rsa4096 2018-10-15 [SC] [expires: 2025-05-17]
      D8BA D4DE 7EE1 7AF5 2A83  4B2D 0BB7 5829 C2D4 E821
uid           [ unknown] Brave Software <support@brave.com>
sub   rsa4096 2019-10-17 [S] [expires: 2024-05-17]

/etc/apt/trusted.gpg.d/jtaylor_ubuntu_keepass.gpg
-------------------------------------------------
pub   rsa1024 2011-01-01 [SC]
      57A0 E8DE A026 F8D8 173E  90A5 7858 0881 58B8 0F90
uid           [ unknown] Launchpad PPA for Julian Taylor

/etc/apt/trusted.gpg.d/phoerious-ubuntu-keepassxc.gpg
-----------------------------------------------------
pub   rsa4096 2017-10-23 [SC]
      D89C 66D0 E31F EA28 74EB  D205 6192 2AB6 0068 FCD6
uid           [ unknown] Launchpad PPA for Janek Bevendorff

/etc/apt/trusted.gpg.d/ubuntu-keyring-2012-cdimage.gpg
------------------------------------------------------
pub   rsa4096 2012-05-11 [SC]
      8439 38DF 228D 22F7 B374  2BC0 D94A A3F0 EFE2 1092
uid           [ unknown] Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>

/etc/apt/trusted.gpg.d/ubuntu-keyring-2018-archive.gpg
------------------------------------------------------
pub   rsa4096 2018-09-17 [SC]
      F6EC B376 2474 EDA9 D21B  7022 8719 20D1 991B C93C
uid           [ unknown] Ubuntu Archive Automatic Signing Key (2018) <ftpmaster@ubuntu.com>

/etc/apt/trusted.gpg.d/yubico-ubuntu-stable.gpg
-----------------------------------------------
pub   rsa1024 2012-10-25 [SC]
      3653 E210 64B1 9D13 4466  702E 43D5 C495 32CB A1A9
uid           [ unknown] Launchpad PPA for Yubico
```

Export the key using the last 8 characters
```bash
sudo apt-key export 33505B88| sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/regolith.gpg
```

## Done
### 2023-11-17
- Add role drawio
### 2023-11-16
- Brave
  - Addons and their configuration files:
    [[Brave#Synchronize Settings and Extensions]]
### 2023-11-15
- Espanso Configuration
- Regolith
  - i3 Settings (ilia config file)
- Gnome Settings
  - Keyboard short cuts for shutter
### 2023-11-13
- Different Software installations if Pop!OS or Ubuntu
- Create separate play home_folders.yml to create folders and download ansible repository from github
- Improve variable handling for roles
  - bashalias
  - folders
  - ...
### 2023-11-12
- Create role cifs-mount
### 2023-11-11
- Create role for Winbox
- Create role for Parcellite
- Create role for Folder Setup
- Remove local plain text vault password file and replace it with a keyring password
- Create new folder structure in home directory
- create repository for all ansible roles
### 2023-11-08
- Create role for
  - timeshift
  - appimagelauncher
  - libreoffice
  - terraform
  - spotify
### 2023-11-07
- Create role for openstackclient
- Create role for wireguard
### 2023-11-06
- Create role for tmux
- Create role for regolith
- Create role for git
### 2023-10-??
- Create role for nala
- Create role for ansible
- Create role for vscode
- Create role for appimage installations
  - Obsidian
  - kDrive
  - yubikey-manager
### 2023-10-26
- Create separate role to install and configure vim
### 2023-10-18
- Created Task
