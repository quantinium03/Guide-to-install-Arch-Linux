# How to install intel graphics drivers for the IGPU

This file basically shows how to install [INTEL](https://wiki.archlinux.org/title/intel_graphics) drivers and use it.

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
1. You can Check what graphic hardware you have by running
~~~
$ lspci -k | grep -A 2 -E "(VGA|3D)"
~~~
2. If you have a really old intel processor be sure to see the [wiki](https://wiki.archlinux.org/title/intel_graphics)
3. I have a 11th gen processor so im install drivers for it.
~~~
$ sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel
~~~
## Step 3 - DRM kernel mode setting
1. Edit mkinitcpio.conf
~~~
$ vim /etc/mkinitcpio.conf
~~~
2. Add
   > MODULES=(i915)

3.Replace i915(intel integrated graphics) with amdgpu if you have amd integrated graphics. If you have no integrated graphics just write the nvidia thingies
4. Run the command and see if there are any warnings stating nvidia. There are other warning too. you just have to know which to ignore and which to not.
~~~
$ mkinitcpio -P
~~~

## Done now go back to the main file and follow the steps after installing the drivers like enabling, unmounting and rebooting
