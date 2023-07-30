# How to install DWM in Arch
1. Make a directory localsrc for all git sourcesif you want
~~~
$ sudo pacman -Syu base-devel libx11 libxft libxinerama freetype2 fontconfig firefox
$ mkdir localsrc/suckless
$ cd localsrc/suckless
$ git clone https://git.suckless.org/dwm
$ git clone https://git.suckless.org/st
$ git clone https://git.suckless.org/dmenu
~~~
2. Installing dwm
~~~
$ cd dwm
$ make
$ sudo make clean install
$ cd..
~~~

3. Installing st
~~~
$ cd st
$ make
$ sudo make clean install
$ cd..
~~~

4.Installing dmenu
~~~
$ cd dmenu
$ make
$ sudo make clean install
$ cd
~~~

## If you are using a display manager you’ll need to create a .desktop file for dwm.
~~~
[Desktop Entry]
Encoding=UTF-8
Name=Dwm
Comment=Dynamic window manager
Exec=/usr/local/bin/dwm
Icon=dwm
Type=XSession
~~~
Put that file in /usr/share/xsessions. If dwm refuses to start, change the full path (/usr/local/bin/dwm) to simply dwm

## Heres the key to the farms - [key](https://dwm.suckless.org/)
