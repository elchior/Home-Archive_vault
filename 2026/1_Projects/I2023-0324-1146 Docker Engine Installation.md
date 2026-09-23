---
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:29:06.866+02:00
---

# I2023-0324-1146 Docker Engine Installation
## Problem description

[[Docker#Ubuntu v3|Docker engine installation]] failed with error:
```bash
Error: Installation has failed.
If you'd like to file a bug report please include '/var/log/nala/dpkg-debug.log'

Error: error processing package docker-ce (--configure):

Error: error processing package docker-ce (--configure):
 installed docker-ce package post-installation script subprocess returned error exit status 1
 installed docker-ce package post-installation script subprocess returned error exit status 1
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for man-db (2.10.2-1) ...

Errors were encountered while processing:
 docker-ce

```

## Cause

## Solution
Run the installation again
```bash
sudo nala install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
