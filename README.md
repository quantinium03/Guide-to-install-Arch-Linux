# Guide-to-install-Arch-Linux-2023
This is a guide to install as well as dual boot Arch Linux with Windows. It can also work to install only Arch. You'll just have to skip some parts that i'll mention. For more updated and informated go to [Arch Linux Wiki](https://wiki.archlinux.org/title/Installation_guide)

## Some prerequisites
1. Disable Secure Boot. It can be enable after installation.
2. Disable Fast Boot
3. Disable Hibernation

## Step 1 - Pre-installation
### 1.1 Prepare an installation medium
1. Install the Latest Arch-Linux ISO file from [Arch ISO](https://archlinux.org/download/)
2. Now we'll use and Etcher tool to burn the iso file into a usb.
3. You can use [Balena Etcher](https://etcher.balena.io/), [Rufus](https://rufus.ie/en/) if you are on windows. If you are on Arch read the arch wiki to create an Arch USB [USB flash installation medium](https://wiki.archlinux.org/title/USB_flash_installation_medium)

### 1.2 - Boot the live environment
1. Plug the USB into a USB port and go into the BIOS by smashing that F2 or F12 or Del button and change the boot priority so that the Arch USB is on number 1. Save and Exit
2. You'll see a grub menu with multiple things. Click Enter on the Arch linux installation medium (most probably the top one)
3. You'll see many thing just going saying ok. See if anything failed. After Some time you'll boot into the live environment.
4. You will be logged in on the first virtual console as the root user,

### 1.3 Set the console keyboard layout and font
1. Set your Keyboard Layout. For example to set the German Layout
~~~
$ loadkeys de-latin1
~~~
2 . 
