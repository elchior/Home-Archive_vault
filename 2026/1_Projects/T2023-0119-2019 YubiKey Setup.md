---
tags: [state/closed/2023/1]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:32:49.494+02:00
---

# T2023-0119-2019 YubiKey Setup
#state/closed/2023/1
## Description
I bought two Yubikey 5 NFC which need to be configured now for as many services as possible

## Todo

## Done
### 2023-01-18
- Created Task

Primary and Secondary key registration on each website
Instructions see <https://www.yubico.com/ch/setup/yubikey-5-series/>

Configured services
- [x] Google
- [x] Coinbase
- [x] Facebook
- [x] Protonmail
- [x] Twitter
- [x] GitHub

### 2023-01-19
Configured services
- [x] Instagram
  - Requires Yubico Authenticator
  - Configuration only on smartphone with app possible.
- [x] KeePassium
      see <https://keepassium.com/articles/how-to-use-yubikey/>
  - Requires
    - KeePassium Pro
    - YubiKey Manager (to change second slot to challenge response)
      - Generate a secret key for the primary yubikey and copy it to be used for the secondary yubikey. Now both can be used to unlock the keepass db.
- [x] KeePassXC
      see <https://keepassxc.org/docs/#faq-yubikey-howto>
  - Requires YubiKey Manager see KeePassium
  - In KeePassXC Database > Security > Add additional protection... > YubiKey Slot2
- [x] Ubuntu
     <https://support.yubico.com/hc/en-us/articles/360016649099-Ubuntu-Linux-Login-Guide-U2F>
  - Requires libu2f-udev
- Documentation [[YubiKey]]

### 2023-01-24 ?
- [x] SSH
      <https://developers.yubico.com/SSH/>
      Documentation: [[YubiKey#FIDO2 resident SSH Key (Discoverable Key)]]
