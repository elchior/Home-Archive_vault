---
tags: [state/closed/2023/06/26]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:34:22.116+02:00
---

# T2023-0625-1509 Bluetooth Audio Issue
#state/closed/2023/06/26
## Description
Bluetooth did work fine with any headset but it was not possible to send audio output to the bluetooth headsets.

## Todo

## Done
### 2023-06-25
- Created Task

Install PipeWire
```bash
sudo apt install libldacbt-{abr,enc}2 libspa-0.2-bluetooth pipewire-audio-client-libraries libspa-0.2-jack
sudo apt install wireplumber
```
Enable PipeWire and disable PulsAudio
```bash
systemctl --user --now enable wireplumber.service
systemctl --user --now disable pulseaudio.{socket,service}
systemctl --user mask pulseaudio
```
Configure PipeWire
```bash
sudo cp -vRa /usr/share/pipewire /etc/
```
Start WirePlumber service
```bash
systemctl --user --now enable wireplumber.service
```

Restart system
