# How to install qemu

## Step 1 - Setting up Virtual Machines in QEMU on Arch Linux
QEMU/KVM Dependancies to install:
```
$ sudo pacman -Syu     // update your system
$ sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat ebtables iptables libguestfs
```
## Step 2 - Make Libvirt Group for your user
edit /etc/libvirt/libvirtd.conf
~~~
$ vim /etc/libvirt/libvirtd.conf
~~~
add the following
~~~
unix_sock_group = "libvirt"
unix_sock_rw_perms = "0770"
~~~
Now add the user and create group
~~~
$ sudo usermod -a -G libvirt $(whoami)
$ newgrp libvirt
~~~
Now reboot
~~~
$ reboot
~~~

# How to use qemu 
### Step 1 - To create a virtual image use:
```
$ qemu-img create -f qcow2 Image.img 10G
```
create is to create an image, -f qcow2 sets the format to qcow2, Image.img is our final file and 10G is it's size

### Step 2 Launching the VM:
```
$ qemu-system-x86_64 -enable-kvm -cdrom OS_ISO.iso -boot menu=on -drive file=Image.img -m 2G
```
-enable-kvm enables KVM, -cdrom selects an iso to load as a cd, -boot menu=on enables a boot menu, -drive file= selects a file for the drive, -m sets the amount of dedicated RAM)
(Remember! Ctrl + Alt + G to exit capture, Ctrl + Alt + F to fullscreen!


Basic performance options
1. -cpu host (sets the CPU to the hosts' CPU)
2. -smp 2 (sets the numbers of cores)

Basic Graphics Acceleration

the -vga option can be used to specify one of various vga card emulators:

"qxl" offers 2D acceleration but requires kernel modules "qxl" and "bochs_drm" to be enabled:

-vga qxl

"virtio" works much better and supports some 3D emulation:

-vga virtio -display sdl,gl=on


