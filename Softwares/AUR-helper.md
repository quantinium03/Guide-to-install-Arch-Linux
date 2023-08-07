# How to install yay or paru i.e. AUR helpers 
Pacman, the default package manager on Arch-based distros is powerful, but it lacks the functionality of downloading packages from the Arch User Repository [AUR](https://aur.archlinux.org/). The AUR is a community-maintained repository providing thousands of third-party packages in the form of installation scripts, also known as PKGBUILDs. To install packages using these PKGBUILDs, you need to use an AUR helper like Yay. Yay doesn't come preinstalled on Arch Linux and isn't available in the official Arch repositories 
1. Update your system
```
$ sudo pacman -Syyu
```
2. We'll are gonna git clone a repo that contains yay build package. I like to put all my cloned repo in a folder named localsrc. You can name yours as you want or dont create it at all its your choice. Having it just makes its easier to know which repo's are cloned.
```
$ mkdir localsrc
$ cd localsrc
$ sudo pacman -S --needed git base-devel // no need to install if already installed.
$ git clone https://aur.archlinux.org/yay-bin.git
$ cd yay-bin
$ makepkg -si
```
## How to use yay

### Searching Packages
~~~
$ yay searchterm         // like yay brave-bin
~~~

### Installing Packages
~~~
$ yay -S brave-bin
~~~
### Removing Packages
~~~
yay -Rsn brave-bin
~~~
-R = for removing and -ns is to remove the package along with its dependencies,

### Updating AUR packages 
~~~
$ yay -Sua              //To only update AUR packages, use the -Sua flag with the command:
~~~

### Help 
~~~
$ yay --help
~~~
### For man pages
~~~
$ man yay
~~~


You can install [paru](https://github.com/Morganamilo/paru) if you dont wanna install yay.

~~~
$ sudo pacman -S --needed base-devel
$ git clone https://aur.archlinux.org/paru.git
$ cd paru
$ makepkg -si
~~~

Other AUR helper/tools: 
1. [pikaur](https://github.com/actionless/pikaur)
2. [aurutils](https://github.com/AladW/aurutils)
