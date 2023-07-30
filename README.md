# Guide-to-install-Arch-Linux-2023
This is a guide to install as well as dual boot Arch Linux with Windows. It can also work to install only Arch. You'll just have to skip some parts that i'll mention. For more updated and informated go to [Arch Linux Wiki](https://wiki.archlinux.org/title/Installation_guide)

## Some prerequisites
1. Disable Secure Boot. It can be enable after installation.
2. Disable Fast Boot
3. Disable Hibernation.
4. If You are dual booting. Check if the EFI or the boot partition of windows is more than 100MB as it may cause problems. Refer to [Wiki](https://wiki.archlinux.org/title/Dual_boot_with_Windows) to increase partition size or see a video on youtube.

** DON'T TYPE THE DOLLAR SIGNS IT IS JUST FOR SHOWCASING THAT IT IS A COMMAND. **

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

### 1.3 Set the console keyboard layout, font and basic stuff
1. The default Keymap is US. Available layouts can be listed with:
~~~
$ ls /usr/share/kbd/keymaps/**/*.map.gz
~~~
2. Set your Keyboard Layout. For example to set the German Layout. For more info [loadkeys](https://man.archlinux.org/man/loadkeys.1)
~~~
$ loadkeys de-latin1
~~~
3 . The Default font size is kinda small. So you might want to increase the size by 
~~~
$ setfont ter-132b
~~~
4. Connect to the internet,
  
   * Ensure your network interface is listed and enabled
      ~~~
       $ ip link
      ~~~
   * For wireless and WWAN, make sure the card is not blocked with [rfkill](https://wiki.archlinux.org/title/Network_configuration/Wireless#Rfkill_caveat).
   * Ethernet—plug in the cable.
   * Wi-Fi—authenticate to the wireless network using [iwctl](https://wiki.archlinux.org/title/Iwd#iwctl).
     ~~~
     $ iwctl
     [iwd]# device list
     [iwd]# station device scan
     [iwd]# station device get-networks
     [iwd]# station device connect SSID
     [iwd]# exit
     ~~~
     Replace device with the interface you wanna connect (For me it was wlan0) and SSID with the name of you network. after connecting it'll ask you the passphrase. It is the password of your WIFI.
   * Check if it is connected by pinging a website
     ~~~
     $ ping archlinux.org
     ~~~
5. Update the system clock. Use timedatectl(1) to ensure the system clock is accurate:
~~~
$ timedatectl
~~~

### 1.4 Verify the boot mode
1. Type the following command to see is you are booted in UEFI mode. 
~~~
$ cat /sys/firmware/efi/fw_platform_size
~~~
If the command returns 64, then system is booted in UEFI mode and has a 64-bit x64 UEFI. If the command returns 32, then system is booted in UEFI mode and has a 32-bit IA32 UEFI; while this is supported, it will limit the boot loader choice to GRUB. If the file does not exist, the system may be booted in BIOS (or CSM) mode. Most probably for a normal user you'll want a 64-bit system so refer to the motherboard manual for this. If the file doesn't exist see if your computer supports UEFI mode by going into the BIOS. If it is enabled and it's still not showing recreate the USB by choosing a GPT partition type and UEFI enabled.

### Partitioning the disks
1. Use lsblk to see your drives.It can be named sda,sdb,nvem0n1,nvme1n1. You gotta know which drive you wanna install arch on as we are gonna wipe the drive afterwords so if you by mistake choose some other drive with important data on it, you are gonna regret it. Just a WARNING.
~~~
$ lsblk
~~~
2.You can use fdisk command to get information about your drive partitions. its helpful as one time while dual booting i had a 675MB EFI partition and 674MB windows recovery partition. you can see the partition info with it inorder to differ EFI Partition that is FAT32 from recovery partition.
~~~
$ fdisk -l
~~~
3. Here i'm gonna make partition for boot (for the bootloader and not needed if dualbooting ), swap (its a partition that will work as ram if we overload our ram or if you wanna use hibernation you'll need it), root (it contains our system packages) and home (it will contain all the stuff like docs, downloads, pictures, etc in it). Some people dont like to make swap as most of the time its useless if you dont wanna hibernate as well as not making the home and using the root partition as home only. You can choose whatever you want but im gonna make all four partitions.
4. For my purpose im gonna choose sda as my drive to install. Your drive may be different so choose carefully.
~~~
$ cfdisk /dev/sda
~~~
5. It'll open a menu thatll show your partition. To move we'll use the arrow keys. Now press ENTER on the unallocated space.
   * We are gonna make the first partition 512MiB. You dont need to make it if you are dual booting. Press ENTER. Now with down arrow key move to next unallocated space.
   * We'll make the swap around 16GiB
