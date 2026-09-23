---
tags: [state/closed/2022]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:30:09.126+02:00
---

# T2022-0711-1257 Technologies and Tools
#state/closed/2022

## Description
Check out the technologies and tools to use for home automation.

## Conclusion
- Virtualization
  - [[Proxmox]]
     Proxmox is set be cause I know it already well from the office.
- Deployment (creating VM templates)
  - [[Packer]]
     Not as useful as I was hoping for. The Proxmox plugins are limited to create deployments with the live iso from Ubuntu.
    - This creates quit bit guests, no minimal installation possible.
    - It is very slow, compared to the cloud image deployment.
    - The Ubuntu images do not contain new updates since the last major release. This seems to be different for example for Debian.
  - (Ubuntu) Cloud image ([[Cloud Image Script]]) with [[cloud-init]]
     Fast deployment possible with the cloud-image.bsh script.
    - Needs a script executed on the Proxmox server. (maybe using Ansible to do that?)
    - Fast creation of the template.
    - Latest updates included.
    - Smaller image. There is even a minimal cloud image.
  - Conclusion
    - cloud image seems to work better/faster (at least for Ubuntu)
    - Maybe not all distros support cloud image. Therefore Packer may still be an option for those.
    - Both need to create VMs from the deployed templates which is done on the Proxmox VE host GUI or with the script [[Clone VM Template Script]]
- Automation
  - [[Ansible]]
  - [[Terraform]]

## Todo
## Done
### 2022-07-11
- Created Task
