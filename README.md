# Chuwi Vi8

[![Join the chat at https://gitter.im/Manouchehri/vi8](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Manouchehri/vi8?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

The goal of this project is basically to improve support at all levels, Linux or Windows related. The primary focus is to keep the amount of noise to a minimum.

## Finding files

TODO (currently waiting on a few emails)

## Current Status: Native support by 

Known Operating Systems to work off of:

### Windows 8.1 (native)

#### Issue(s)
- Sketchy driver signing (or lack of)

### Windows 10 (clean install or upgrade; use existing Windows 8.1 drivers for a clean install)

#### Issues(s)
- Must use 32-bit builds despsite having a 64-bit CPU (will not fix, ever)
- Sketchy driver signing (or lack of)

### Debian 

#### Working

- WiFi

#### Issues(s)
- Confirmed to work with [multi-arch testing installer](http://cdimage.debian.org/cdimage/weekly-builds/multi-arch/iso-cd/debian-testing-amd64-i386-netinst.iso)
- Recent multi-arch should be merged into Jessie by now (installer only, not LiveUSB).

##### Missing Drivers

- No touchscreen
- No Bluetooth
- No audio
- No webcams

Check issue tracker for details on various other problems that have yet to be solved (touch, WiFi, etc).
