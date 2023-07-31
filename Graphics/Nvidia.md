# How to install NVIDIA-DKMS (Proprietary drivers) in Arch Linux.
This file basically shows how to install [NVIDIA](https://wiki.archlinux.org/title/NVIDIA) drivers and use it.  If you have hybrid Intel/NVIDIA graphics see [NVIDIA OPTIMUS](https://wiki.archlinux.org/title/NVIDIA_Optimus).

## Step 1 - Getting things ready 
1. Firstly update your system
~~~
$ sudo pacman -Syu
~~~
2. Edit your pacman.conf
~~~
$ vim /etc/pacman.conf/
~~~
3. Uncomment VerbosePkgLists and ParallelDownloads = 3
4. You can also add ILoveCandy under the ParallelDownloads for a surprise. You can remove it if you dislike it.
~~~
VerbosePkgLists
ParallelDownloads = 3
ILoveCandy
~~~
6. ALso Uncomment
~~~
[multilib]
Include = /etc/pacman.d/mirrorlist
~~~
7. Update your system again.
~~~
$ sudo pacman -Syu
~~~

## Step 2 - Installing the drivers
1. This step might be a bit confusing. First find your [nvidia card from this list here](https://nouveau.freedesktop.org/CodeNames.html)
2. You can Check what graphic hardware you have by running
~~~
$ lspci -k | grep -A 2 -E "(VGA|3D)"
~~~
4. Check what driver packages you need to install from the list below

| Driver name  | Base driver | OpenGL | OpenGL (multilib) |
| ------------- | ------------- | ------------- |  ------------ | 
| Maxwell (NV110) series and newer  | nvidia | nvidia-utils | lib32-nvidia-utils |
| Kepler (NVE0) series  | nvidia-470xx-dkms  | nvidia-470xx-utils | lib32-nvidia-470xx-utils |
| GeForce 400/500/600 series cards [NVCx and NVDx] | nvidia-390xx  | nvidia-390xx-utils  | lib32-nvidia-390xx-utils |
5. For more info it'll be better to go see the [wiki](https://wiki.archlinux.org/title/NVIDIA)
6. I have a 30-series card that is newer than the Maxwell Series so i'm gonna install the nvidia driver.
7. In Linux we have 5 [kernels](https://wiki.archlinux.org/title/kernel) - Mainline(Linux),linus-lts, linux-hardened, Realtime(linux-rt, linux-rt-lts), linux-zen.
8. I'll choose the mainline linux kernel for now. i will explain how to install other linux kernel on some other day.
9.. Nvidia drivers you are gonna install basically depends on which kernel you are using. For example Nvidia for Linux, Nvidia-lts for linus-lts, nvidia-dkms for all other kernels.
10.. im gonna install the nvidia-dkms even though i have linux kernel for now. it has no problem adapting to it. you just cant install nvidia for any other kernel other than mainline(linux) or nvidia-lts for any other kernel than linux-lts.
11. So now on the main part of installing the drivers. **Have the linux-headers installed as its needed by these packages. linux-headers-lts forlinux-lts**
~~~
$ sudo pacman -S nvidia-dkms libglvnd nvidia-utils opencl-nvidia lib32-libglvnd lib32-nvidia-utils lib32-opencl-nvidia nvidia-settings
~~~
12. Now configuring to make the drivers work and updating them automatically
## Step 3 - DRM kernel mode setting
1. Edit mkinitcpio.conf
~~~
$ vim /etc/mkinitcpio.conf
~~~
2. Add
   > MODULES=(i915 nvidia nvidia_modeset nvidia_uvm nvidia_drm)

3.Replace i915(intel integrated graphics) with amdgpu if you have and integrated graphics. If you have no integrated graphics just write the nvidia thingies
4. Run the command and see if there are any warnings stating nvidia. There are other warning too. you just have to know which to ignore and which to not.
~~~
$ mkinitcpio -P
~~~
5. Edit the grub file
~~~
$ vim /etc/default/grub
~~~
6. Edit the following line
~~~
GRUB_CMDLINE_LINUX="nvidia_drm.modeset=1 rd.driver.blacklist=nouveau modprob.blacklist=nouveau"
~~~
7. Run the following command
~~~
$ sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
8. Adding a pacman hooks
~~~
$ /etc/pacman.d/hooks/nvidia.hook
---------------------------------------------------------
// Add it to to the file
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
Target=nvidia-dkms
Target=linux
# Change the linux part above and in the Exec line if a different kernel is used

[Action]
Description=Update Nvidia module in initcpio
Depends=mkinitcpio
When=PostTransaction
NeedsTargets
Exec=/bin/sh -c 'while read -r trg; do case $trg in linux) exit 0; esac; done; /usr/bin/mkinitcpio -P'
~~~
9. Make sure the Target package set in this hook is the one you have installed in steps above (e.g. nvidia, nvidia-dkms, nvidia-lts or nvidia-ck-something).
10. **Note :**  The complication in the Exec line above is in order to avoid running mkinitcpio multiple times if both nvidia and linux get updated. In case this does not bother you, the Target=linux and NeedsTargets lines may be dropped, and the Exec line may be reduced to simply Exec=/usr/bin/mkinitcpio -P.

## Step 4 Now reboot
~~~
$ systemctl enable NetworkManager.service
$ exit
$ umount -lR /mnt        //unmount all the partition
$ reboot
~~~

### Enjoy.
Credits -
1. [Install Hyprland Arch Linux on Laptop with Nvidia RTX GPU](https://www.youtube.com/watch?v=_deaeSU1WK8&ab_channel=Ja.KooLit)
2. [Muta's Arch Linux Build](https://www.youtube.com/watch?v=M_f8pnXIrF8&ab_channel=%E5%AD%A3%E6%9F%93) - [SomeOrdinaryGamers](https://www.youtube.com/@SomeOrdinaryGamers)
3. [The Holy Arch Bible](https://wiki.archlinux.org/title/NVIDIA)
