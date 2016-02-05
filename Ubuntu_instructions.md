# Ubuntu installation and instructions
This document will instruct you to install Ubuntu 15.10 on the Chuwi Vi8 tablet. To make it boot on its own, and add support for hardware.

## Requirements
* Chuwi Vi8
* 3 port USB OTG hub [Recommended](http://www.ebay.com/itm/CABLE-FOR-ANDROID-TABLET-COMPUTER-3-PORT-USB-2-0-OTG-HUB-OTG-CHARGING-CABLE-/331639050796?)
* USB Keyboard
* 2GB USB Memory stick
* Access to Internet

### Recommended
* Separate working computer with access to the Internet *and*
* A second USB stick *or*
* **USB Ethernet or WiFi adapter** *(Can skip the tedious step [Installing 32bit Grub](https://github.com/Manouchehri/vi8/blob/master/Ubuntu_instructions.md#installing-32bit-grub) )*

## What works?

- [x] Booting live from USB
- [x] Installing
- [x] Graphics acceleration
- [x] WiFi
- [x] Bluetooth
- [x] Touch
- [ ] Audio
- [ ] Accelerometer
- [ ] Lock and volume keys (long press as power works)
- [ ] Some freezes 

## Installation
###UEFI (BIOS) settings

1. Shut down the tablet completely
2. Connect a keyboard via the USB OTG hub or the adapter that shipped with the tablet.
3. Press and hold the lock button for a period of time to boot the tablet, simultaneously press the `Esc` key to display the UEFI menu.

#### Disable secure boot
4. Use the keyboard arrows or touch screen to select `Secure Boot Option`
5. Disable Secure Boot
6. Press `F10` to save and exit this menu

#### Set max Cstate to 1
We will disable a power saving feature of the CPU, this will remove most freezes. This will likely be fixed in the future and it works as intended on some older kernels. But then other features are problematic.

7. Enter SCU (its the regular UEFI settings)
8. Go to power and Advanced CPU Control
9. Set C-States to `C1`
10.  Press `F10` to save and exit this menu


### Prepare installation media
**Download Ubuntu 16.04**, at the time of writing you can download the **[daily image](http://cdimage.ubuntu.com/daily-live/current/)** or after its released you can go to [Ubuntu.com](ubuntu.com). You want the AMD64 version. If you do use the Daily-live image and have working internet during installation (USB ethernet or WIFI) you will not have to fix a broken bootloader after a failed but otherwise functioning Ubuntu install.

If you run Ubuntu on your separate machine use the Startup Disk Creator. 

If you use Windows I suggest [Rufus](https://rufus.akeo.ie/), select GPT partition scheme after you selected the ISO.

If you use OS X I suggest googling how because its a bit more work than the other operating systems.

After that you need to copy over a 32Bit GRUB over to the memory stick. [Download it from here](https://github.com/Manouchehri/vi8/blob/master/bootia32.efi?raw=true) and put it into EFI/BOOT/

The last step is to change some grub settings, so [download this file](https://raw.githubusercontent.com/Manouchehri/vi8/master/Ubuntu_support_files/grub.cfg?raw=true) and overwrite /boot/grub/grub.cfg

### Booting into live
Use the USB OTG hub and connect at least a keyboard and the USB stick you just prepared. Shut down the tablet if its powered on and start it again by holding down the lock button for a while and press the `Esc` key on the keyboard to enter the UEFI menu. Use the mouse, touchscreen or keyboard to select the Boot Manager, then use the keyboard to select your USB stick. If you can’t see the USB stick, the 32Bit GRUB binary is either damaged or not correctly placed on the USB stick.

Boot by selecting Try Ubuntu

### Installation
You can fairly easily do this without a mouse by using TAB, space, Arrow and Enter on the keyboard. If you have to few USB ports for both a mouse and a keyboard you can swap between them. Open the application called Install Ubuntu 15.10 and follow the installation wizard. **If you have a working internet connection via a USB device, tick the box to download updates** I suggest until more progress on hardware and configuring Ubuntu is made to install without a swap partition, and login automatically. For me it looks like disabling swap removes nearly all freezes, and I have not figured out how to make touch work on the login screen.

If you want to learn more about installing Ubuntu or similar distributions I suggest looking at guides on YouTube, I may go into more detail here when the finished installation is more user friendly.

### Installing 32Bit GRUB
**If you installed using a USB Ethernet adapter or Wifi dongle, skip this step.**

On your separate working computer download [grub-efi-ia32-bin](http://packages.ubuntu.com/wily/amd64/grub-efi-ia32-bin/download) and put it on your second USB stick. Then boot the tablet with the first USB stick in the USB hub and hit `Esc` on the keyboard to go into the UEFI menu. Now go to Boot Menu and then select your USB stick. This time we are going to use this GRUB to boot into the Ubuntu installation we already installed. So hit “C” on your keyboard to go into the grub shell. I will describe the following steps in a manner to so you will understand how to figure out what to write if there are some differences to your system.

Enter the command 

```
ls
```

You will see a list of hard drives and partitions, we need to find the partition where Ubuntu got installed. For me that was (hd1,gpt2) you can check if this is the correct one by typing

```
ls (hd1,gpt2)/
```
You will either see “Unknown filesystem.” or you will see a list of files and folders that are on that partition. If you see a file called `autorun.inf` or a folder called `casper/` its would be the USB stick, so try another partition. If you see a file called `vmlinuz` and one called `ignited.img` you found the correct pairtion.

Next we need to find out the device name that Linux will call this partition. So use the following command, or replace out the name of the hard drive or partition if you need.

```
cat (hd1,gpt2)/etc/fstab
```
You will see the contents of the text file called fstab the interesting information is in a line that is commented out (starting with #) the line you want to look for says `/ was mounted on` and then it says the device name of the partition that Linux would call this. for me that was `/dev/mmcblk0p2` we need this information in the next command.

The next command to write a few commands to actually boot. For the `linux` command the first parameter has to locate the vmlinuz and needs the harddisk and partition information that we found earlier, the `video` parameter to get 3D acceleration working, and a `root` with the device information we gathered earlier. the `initrd` command also needs the partition information to find the initrd.img and lastly the `boot` command to boot so do the following commands or change to fit your hardware.
```
linux (hd1,gpt2)/vmlinuz video=1280x800@60 root=/dev/mmcblk0p2
initrd (hd1,gpt2)/initrd.img
boot
```

You have now booted into the Ubuntu we installed on the internal storage. You can remove the first USB stick. You should have already prepared a second USB stick with the deb file downloaded earlier. After logging in to Ubuntu you can plug in the second USB stick and double click on the deb file, then select install. You will be prompted to enter you admin password, the one you selected in the install wizard for your user will work. 

### Chaining Grub settings
**If you installed using a USB Ethernet adapter or WiFi dongle, first time you boot you need to press** `Esc` **in the Grub menu, then press** `e` **to edit the boot script and replace** `quiet splash` **with** `video=1280x800@60` **then press** `F10` **to boot. Then after the successful boot** 

We need to edit the file `/boot/grub/grub.cfg` I suggest we use the text editor nano, so open up the terminal application and type
```
sudo nano /etc/default/grub
```
enter your password and hit enter

Find the line starting with `GRUB_CMDLINE_LINUX_DEFAULT` and add the parameter `video=1280x800@60` into the quotation marks. My line then looks like this `GRUB_CMDLINE_LINUX_DEFAULT=“video=1280x800@60”` then save by pressing “CTRL+X” then press “Y” then press “Enter” to confirm. The last step now is to enter the following command
```
sudo update-grub
```
and supply your password if needed. You should now be done and have an Ubuntu that is able to boot it self.

## WiFi
I will not go into detail right now, but https://github.com/hadess/rtl8723bs driver works. You can 

```
sudo apt-get -y install git
```
Enter your password and hit enter
```
cd
git clone https://github.com/hadess/rtl8723bs.git
cd rtl8723bs
make
sudo make install
sudo reboot
```

## Bluetooth
The WiFi drivers indeed are also drivers for Bluetooth, but getting Bluetooth working is a bit difficult. You need to patch your kernel with [this patch](https://raw.githubusercontent.com/Manouchehri/vi8/master/Ubuntu_support_files/rfkill.patch). Then use [this program](https://github.com/lwfinger/rtl8723bs_bt) to add the firmware to your Linux install and also there is a script that needs to be run each boot to turn on the Bluetooth module.

## Touch
I will not go into detail right now, but https://github.com/onitake/gslx680-acpi driver works. You can set your screen orientation to clockwise, then

```
sudo apt-get -y install git
```
Enter your password and hit enter
```
git clone https://github.com/onitake/gslx680-acpi.git
wget -O silead_ts.fw https://github.com/Manouchehri/vi8/blob/master/Ubuntu_support_files/silead_ts.fw?raw=true
sudo mv silead_ts.fw /lib/firmware/
cd gslx680-acpi
make
sudo make install
```

Now reboot or `sudo insmod gslx680_ts_acpi.ko`

##Further research
I intend to follow up and more devices work. Adding them to this guide. Please feel free to help out.
