# This includes almost all packages i install on my desktop.

## 1. Office 
1. There are many of office utilities. Heres a [list](https://wiki.archlinux.org/title/Category:Office)
2. In my daily usage i prefer [LibreOfiice](https://wiki.archlinux.org/title/LibreOffice).
~~~
$ sudo pacman -S libreoffice-fresh
~~~
There are two libreoffice -
  * [libreoffice-still](https://archlinux.org/packages/?name=libreoffice-still) is the stable maintenance branch with relatively rare updates, for conservative users.
  * [libreoffice-fresh](https://archlinux.org/packages/?name=libreoffice-fresh) is the feature branch, with new program enhancements for early adopters or power users.

## 2. Browsers
1. There are any browsers. Heres the [list](https://wiki.archlinux.org/title/List_of_applications/Internet#Web_browsers).
2. My Go-To for now are firefox and brave. Firefox for daily. Sometimes and idk why some websites dont work on firefox so brave.
~~~
$ sudo pacman -S firefox
$ yay -S brave-bin             // If you wanna compile it you can use brave-git or brave.
~~~
3. if you wanna install tor use >$ sudo pacman -S torbrowser-launcher
### I got a cool list on privacy status of stuff like browsers, web engines, etc. [Spyware Watchdog Article Catalog](https://xgqt.gitlab.io/spywarewatchdog/articles/index.html)
