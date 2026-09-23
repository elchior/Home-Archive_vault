---
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:29:12.221+02:00
---

# I2022-0916-0841 Pfsense DNS problem
## Problem description
DNS Resolution for the firewall it self (pfsense.home.arpa) as well as other internal and also external systems did not work any more.

## Cause
Not sure yet...

## Solution
- I disabled ipv6 since I don't want to use that. But this did not seem to solve the issue.
- What solved the problem, was to enable the firewall to overwrite the DNS I have configured from swisscom with the ones given by dhcp.
  see [[pfSense#DNS]]
- I have removed that setting again and it still works now... Maybe pfsense was blocked from the swisscom DNS server for some reason some where.
