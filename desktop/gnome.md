# How to install GNOME on Arch

### 1. If you want to install gnome everything.
~~~
$ sudo pacman -Syu xorg xorg-server gnome gnome-tweaks nautilus-sendto gnome-nettool gnome-usage gnome gnome-multi-writer adwaita-icon-theme xdg-user-dirs-gtk fwupd arc-gtk-theme seahosrse gdm
$ systemctl enable gdm
$ reboot
~~~

### 2. If you want to go minimal
~~~
$ sudo pacman -Syu xorg xorg-server gnome-shell nautilus gnome-terminal guake gnome-tweak-tool gnome-control-center xdg-user-dirs gdm
$ sudo systemctl enable gdm
$ reboot
~~~
**Beware** somethings may not work out of the box

### 3, if you want to go the middle ground
~~~
$ sudo pacman -Syu gnome gdm
$ sudo systemctl enable gdm
$ reboot
~~~
