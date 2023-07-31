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
2. Check what driver packages you need to install from the list below

| Driver name  | Base driver | OpenGL | OpenGL (multilib) |
| ------------- | ------------- | ------------- |  ------------ | 
| Maxwell (NV110) series and newer  | nvidia | nvidia-utils | lib32-nvidia-utils |
| Kepler (NVE0) series  | nvidia-470xx-dkms  | nvidia-470xx-utils | lib32-nvidia-470xx-utils |
| GeForce 400/500/600 series cards [NVCx and NVDx] | nvidia-390xx  | nvidia-390xx-utils  | lib32-nvidia-390xx-utils |
