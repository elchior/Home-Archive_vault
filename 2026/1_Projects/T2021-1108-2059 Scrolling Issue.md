---
tags: [state/closed/2021/11/15]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:29:19.702+02:00
---

# T2021-1108-2059 Scrolling Issue
#state/closed/2021/11/15

## GOAL
since installation of i3wm scrolling with the touchpad is the wrong way round.

## ACTION
Add Tapping and NaturalScrolling to the file /usr/share/X11/xorg.conf.d/40-libinput.conf
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        Option "NaturalScrolling" "true"
        Option "Tapping" "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```

### 2021-11-08
Need to check if this works after reboot.
### 2021-11-15
It works :-)
