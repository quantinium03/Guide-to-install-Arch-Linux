# Guide-to-install-Arch-Linux-2023
This is a guide to install as well as dual boot Arch Linux with Windows. It can also work to install only Arch. You'll just have to skip some parts that i'll mention. For more updated and informated go to [Arch Linux Wiki](https://wiki.archlinux.org/title/Installation_guide)

## Some prerequisites
1. Disable Secure Boot. It can be enabled after installation.
2. Disable Fast Boot
3. Disable Hibernation.
4. If You are dual booting. Check if the EFI or the boot partition of windows is more than 100MB as it may cause problems. Refer to [Wiki](https://wiki.archlinux.org/title/Dual_boot_with_Windows) to increase partition size or see a video on youtube.

**DON'T TYPE THE DOLLAR SIGNS IT IS JUST FOR SHOWCASING THAT IT IS A COMMAND.**

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

### 1.5 Partitioning the disks
1. Use lsblk to see your drives.It can be named sda,sdb,nvem0n1,nvme1n1. You gotta know which drive you wanna install arch on as we are gonna wipe the drive afterwords so if you by mistake choose some other drive with important data on it, you are gonna regret it. Just a WARNING.
~~~
$ lsblk
~~~
2.You can use fdisk command to get information about your drive partitions. its helpful as one time while dual booting i had a 675MB EFI partition and 674MB windows recovery partition. you can see the partition info with it inorder to differ EFI Partition that is FAT32 from recovery partition.
~~~
$ fdisk -l
~~~
3. Here i'm gonna make partition for boot (for the bootloader and not needed if dualbooting ), swap (its a partition that will work as ram if we overload our ram or if you wanna use hibernation you'll need it), root (it contains our system packages) and home (it will contain all the stuff like docs, downloads, pictures, etc in it). Some people dont like to make swap as most of the time its useless if you dont wanna hibernate as well as not making the home and using the root partition as home only. You can choose whatever you want but im gonna make all four partitions.
4. For my purpose im gonna choose sda as my drive to install which is 1TB. Your drive may be different so choose carefully.
~~~
$ cfdisk /dev/sda
~~~
5. It'll open a menu thatll show your partition. To move we'll use the arrow keys. Now press ENTER on the unallocated space.
   * We are gonna make the first partition 512MiB. You dont need to make it if you are dual booting. Press ENTER. Now with down arrow key move to next unallocated space.
   * We'll make the swap around 16GiB. For normal use i think 16GiB is enough even though you have more than 64 GIB of ram. For a proper swap size guide - [How Much Swap] (https://itsfoss.com/swap-size/)
   * Now go to type with arrow keys and select the linux swap.
   * We'll make the root partition 50GiB. Some people prefer 30GiB but for me it always seem less. You can choose whatever size you want just dont go under 30GiB.
   * We'll make the home partition and just press Enter without changing any values. After this i think i should have around 860GiB of home.
   * **IMPORTANT** - Only the swap file should have linux swap written beside it. Other three should have linux filesystem.
   * Now click on write to write the partition table and  then quit.
   * We have Partitioned the drive.
   * Type lsblk again to see the partitioned drive.

### 1.6 Format the partitions
1. For this use case im gonna make the drives with [ext4](https://docs.kernel.org/admin-guide/ext4.html) filesystem. I'll also tell how to make BTRFS filesystem in other file but for now ext4. just understand btrfs give ability to take a snapshot so if we break our system we can roll back.easily and i like btrfs.
2. Lets start formatting
~~~
  $ lsblk
  $ mkfs.fat -F32 /dev/sda1                    // our boot partition. Dont need it if dual boot
  $ mkswap /dev/sda2
  $ swapon /dev/sda2
  $ mkfs.ext4 /dev/sda3
  $ mkfs.ext4 /dev/sda4
~~~
3. Done with formatting the drive. type lsblk you'll see [SWAP] written beside the swap partition.

### 1,7 Mount the drive
~~~
$ mount /dev/sda3 /mnt                          // sda3 is the root partition
$ mkdir /mnt/boot                               // for linux only install. Dont mount the Windows EFI partition now.
$ mkdir /mnt/home
$ mount /dev/sda4 /mnt/home                     //sda4 is the home partition
$ mount /dev/sda1 /mnt/boot/                    //sda1 is the boot partition
~~~
Cool. Done with mounting the drives except the Windows EFI partition.

## Step 2 Installation
### 2.1 Ranking the mirrors and installing base packages
1. Archlinux uses servers to get the packages we need to install. Having good mirrors means packages install faster as well may prevent packages from out of date due to server being out of sync. It is generally not needed because the Archiso runs reflector when you connect to internet so you have a good copy of mirrorlist but it is generally better to resync the mirrors if you haven't done it in long time to remove any out of sync mirrors. [source](https://wiki.archlinux.org/title/Installation_guide#Select_the_mirrors)
2. There are two tools that we can use to rank mirrors and add them to out mirrorlist file. Reflector and Rankmirrors. Use Any one of them acc to your use case.
3. Reflector - Retrieves the latest mirrorlist from the [MirrorStatus](https://archlinux.org/mirrors/status/) page, filters and sorts them by different parameters like speed, last sync, completion %, etc and overwrites /etc/pacman.d/mirrorlist. 
4. Rankmirrors - It fetches the list of the mirrors and ranks them locally based on many parameters. Reflector is good unless you are using private servers.
~~~
$ cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup   // Creates  backup of mirrorlist if something goes wronge
~~~
5. Using Rankmirrors - 
~~~
$ pacman -Sy                                                    // Linking with the mirrors
$ pacman -S pacman-contrib                                      // installing rankmirrors tools for ranking mirrors
$ rankmirrors -n 10 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist
~~~
6. Using Reflector - use reflector --help to see more options.
~~~
$ pacman -Sy                                               // linking with mirrors
$ pacman -S reflector rsync curl
$ sudo reflector --save /etc/pacman.d/mirrorlist --latest 10 --protocol https --sort rate  
~~~

7. Wait for some time. and we have ranked the mirrorlist.
8. If you got errors after updating the mirrorlist, you can revert back the changes with
~~~
$ cp /etc/pacman.d/mirrorlist.backup /etc/pacman/mirrorlist
~~~
9. Now installing the base packages
~~~
$ pacstrap -K /mnt base linux linux-firmware linux-headers base-devel intel-ucode vim git networkmanager dhcpcd bluez bluez-utils wpa_supplicant network-manager-applet
~~~
10. Go for a coffee break while it install if you have slow internet. Oh yeah btw replace "intel-ucode" with "amd-ucode" if you have an amd processor instead of intel.
## Step 3 Configuring the System
### 3.1 FSTAB
1. Now we generate and **fstab** file that contains our UUID(house address) of our drives.
~~~
$ genfstab -U /mnt >> /mnt/etc/fstab
~~~
### 3.2 Chroot
Change root into the new system: 
~~~
$ arch-chroot /mnt
~~~
### 3.3 Timezone
~~~
$ ls /usr/share/zoneinfo/                                     // To show all Regions
$ ls /usr/share/zoneinfo/Canada/                              // To show city in Canada
$ ln -sf /usr/share/zoneinfo/Region/City /etc/localtime       //Replace Region with Country and city with city
$ hwclock --systohc
~~~
### 3.4 Localization 
~~~
$ vim /etc/locale.gen           // Use nano is you want to. Uncomment or delete the # before you language. For me it'll be en_US.UTF-8 UTF-8
$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf
$ export LANG=en_US.UTF-8
~~~
### 3.5 Users and passwords
~~~
$ passwd                        // type the password it'll not show dont fear
$ useradd -m username           // replace username with any name like fish
$ usermod -aG wheel,storage,power username
$ visudo                        // EDITOR=nano visudo if you wanna use nano. 
~~~
1. Uncomment %wheel ALL=(ALL:ALL) ALL
2. Also add
~~~
    Defaults timestamp_timout=0              // just prompts user to type password again after a long time
~~~
3. add users
~~~
$ echo Archfish > /etc/hostname                // replace archfish with your username
$ cat /etc/hostname
$ vim /etc/hosts
~~~
4. Add this in hosts file
~~~
127.0.0.1     localhost
::1           localhost
127.0.1.1     Archfish.localdomain    localhost
~~~
### 3.6 Mounting the boot directry and installing bootloader
~~~
$ mkdir /boot/efi
$ mount /dev/sdb1 /boot/efi            // imagine if sdb1 is the windows boot partition. its is not same as yours
~~~
1. Im gonnause grub bootloader as systemd seems like a hassle. rEFInd seems cool to look but i wanna install with grub. Maybe i'll add how to install other bootloaders but right mow i choose grub so we going with grub.
~~~
pacman -S grub efibootmgr dosfstools mtools os-prober                //ose-prober only if you are dual booting
vim /etc/default/grub
~~~
2. Uncomment > GRUB_DISABLE_OS_PROBER=false
~~~
$ grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
$ grub-mkconfig -o /boot/grub/grub.cfg
~~~

### 3.7 Installing The graphics drivers. Refer to Graphics Directory for installing drivers.

## Step 4 - Finiishing up with live environment
### 4.1 Enabling things and unmounting
~~~
$ systemctl enable NetworkManager.service
$ exit
$ umount -lR /mnt        //unmount all the partition
~~~

### 4.2 reboot
~~~
reboot
~~~
Eject the USB drive and wait for grub bootloader to come. You'll see an Arch Linux entry. Press ENTER to enter Arch. Use your username as your login and the password you set. If you are using wireless connection you'll have to use " $ nmtui " to activate internet as iwctl is not present
# Done with the base installation. Refer to desktop directory to install DE/WM acc to your usage and installation of nvidia drivers would be in nvidia.md
