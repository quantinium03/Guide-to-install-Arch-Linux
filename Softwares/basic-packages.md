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
1. There are many web browsers. Heres the [list](https://wiki.archlinux.org/title/List_of_applications/Internet#Web_browsers).
2. My Go-To for now are firefox and brave. Firefox for daily. Sometimes and idk why some websites dont work on firefox so brave.
~~~
$ sudo pacman -S firefox
$ yay -S brave-bin             // If you wanna compile it you can use brave-git or brave.
~~~
3. if you wanna install tor use
```
$ sudo pacman -S torbrowser-launcher
```
### I got a cool list on privacy status of stuff like browsers, web engines, etc. [Spyware Watchdog Article Catalog](https://xgqt.gitlab.io/spywarewatchdog/articles/index.html)

## 3. Audio Player
I dont generally use any app for local music as i dont have them because i'm lazy to download and play them. Maybe i'll do if i make my own server but for now i use web versions of spotify or spotify desktop. Install [spotify-launcher](https://archlinux.org/packages/?name=spotify-launcher). This package manages a per-user installation in your home directory, allowing Spotify to update itself independently of pacman (similar to how Spotify self-updates on other operating systems).
~~~
$ sudo pacman -S spotify-launcher
~~~

If you prefer to manage Spotify updates with pacman, instead use [spotify](https://aur.archlinux.org/packages/spotify) from AUR which repackages [Spotify for Linux](https://www.spotify.com/us/download/linux/). If you need to add and play local files you need to additionally install [zenity](https://archlinux.org/packages/?name=zenity) and [ffmpeg4.4](https://archlinux.org/packages/?name=ffmpeg4.4). 
~~~
$ yay -S spotify
$ sudo pacman -S zenity ffmpeg4.4
~~~
If you wanna use third-party clients to use spotify check [wiki](https://wiki.archlinux.org/title/spotify). I have only used [spotify-tui](https://github.com/Rigellute/spotify-tui). its really cool. **You'll need spotify premium accounts to use the third party clients**.
