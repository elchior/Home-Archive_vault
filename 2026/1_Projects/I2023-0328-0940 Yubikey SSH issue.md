---
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T22:15:05.369+02:00
---

# I2023-0328-0940 Yubikey SSH issue
## Problem description
Currently the the me-\*\_ecdsa_sk.pu Keys to not seem to work after they have been deployed with the authorized-key role.
```bash
ssh me@192.168.1.172
sign_and_send_pubkey: signing failed for ECDSA-SK "me@hp-me" from agent: agent refused operation
sign_and_send_pubkey: signing failed for ECDSA-SK "me@hp-me" from agent: agent refused operation
me@192.168.1.172: Permission denied (publickey).
```

## Cause
Found several issues and discussions in the forum etc...
<https://github.com/FiloSottile/yubikey-agent/issues/105>
<https://github.com/FiloSottile/yubikey-agent/issues/32>

## Solution

very annoying but I will not use the ssh yubikey at the moment. Will try the setup later again.

Removed ssh keys
```
(base) me@hp-me:~/.ssh$ rm me-primary_ecdsa_sk*
(base) me@hp-me:~/.ssh$ rm me-secondary_ecdsa_sk*
```

Delete yubikey credentials
Secondary key
```bash
(base) me@hp-me:~/.ssh$ ykman fido credentials delete 8ca77763
WARNING: PC/SC not available. Smart card (CCID) protocols will not function.
Enter your PIN: 
Delete ssh:me-secondary openssh openssh (8ca777638d68eeed23c9698ed67d074fdb7f3e2c9111ddfcdca874b0e643381fcf10419bfa8f9aaee573865ed136ce6e)? [y/N]: y

```

Primary key
```bash
(base) me@hp-me:~/.ssh$ ykman fido credentials delete f073564f
WARNING: PC/SC not available. Smart card (CCID) protocols will not function.
Enter your PIN: 
Delete ssh:me-primary openssh openssh (f073564fdee92a7ee4d46eceaf0ac6bed3283af8ed97d5132d4e4203df177bdd40fa4003433a07f17096168317f4fa12)? [y/N]: y
```
