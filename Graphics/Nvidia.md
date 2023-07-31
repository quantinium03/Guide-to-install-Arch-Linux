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

