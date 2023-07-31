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
$ sudo pacman -S mesa lib32-mesa xf86-video-intel vulkan-intel lib32-vulkan-intel
~~~
