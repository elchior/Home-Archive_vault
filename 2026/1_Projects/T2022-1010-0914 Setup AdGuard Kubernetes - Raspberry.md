---
tags: [state/closed/2022/11]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:30:49.870+02:00
---

# T2022-1010-0914 Setup AdGuard Kubernetes - Raspberry
#state/closed/2022/11
## Description
Install an ad guard DNS server to reduce advertising and tracking.

## Todo
## Done
### 2022-10-10
- Created Task
### 2022-11-08
- Try installation AddGuard on the Raspberry MicroK8s Cluster
- Upgraded Raspberry systems

### 2022-11-11
change pfsense dns servers
from
195.186.4.162
195.186.1.162

to
10.1.1.50

Which did not work

Tried quit a lot but the raspi kubernetes cluster did not seem to be really stable. Network access was flapping and failed a lot.
