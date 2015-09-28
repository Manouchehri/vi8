# Chuwi Vi8

[![Join the chat at https://gitter.im/Manouchehri/vi8](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Manouchehri/vi8?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## TODO

- Getting more peripherals to work
- Making it easier for end users to install and use (e.g., prepare .deb packages)

## Current Status: Supported by 

### Windows 8.1 (native) / Windows 10 
 - Both clean installs and upgrades work; use existing Windows 8.1 drivers for a clean install

#### Issue(s)
- Must use 32-bit builds despsite having a 64-bit CPU (see [issue #4](https://github.com/Manouchehri/vi8/issues/4))
- Lack of driver signing for some peripherals

### Debian / Ubuntu

- Confirmed to work with [multi-arch testing installer](http://cdimage.debian.org/cdimage/weekly-builds/multi-arch/iso-cd/debian-testing-amd64-i386-netinst.iso)
- Recent multi-arch should be merged into Debian Jessie by now (installer only, not LiveUSB).
- [Instructions for Ubuntu install and LiveUSB.](https://github.com/Manouchehri/vi8/blob/master/Ubuntu_instructions.md)

#### Working

- WiFi (see [issue #2](https://github.com/Manouchehri/vi8/issues/2))
- 3D acceleration
- USB OTG host

#### Issue(s)

##### Missing Driver(s)
Check issue tracker for ongoing discussions regarding solving these problems.

- No touchscreen
- No Bluetooth
- No audio
- No webcams
- No hardware buttons
- No accelerometer
