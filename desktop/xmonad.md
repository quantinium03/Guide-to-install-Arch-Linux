# How to install Xmonad on arch

If you chose to go this route you would need the [farming tools](https://xmonad.org/INSTALL.html)

If you dont want to do that Here is dotfiles for xmonad by Axarva [dotfiles](https://github.com/Axarva/dotfiles-2.0)
~~~
$ git clone https://github.com/Axarva/dotfiles-2.0.git
$ cd ./dotfiles-2.0
$ chmod +x ./install-on-arch.sh
$ ./install-on-arch.sh
$ sudo ln -s /usr/lib/libasan.so.8 /usr/lib/libasan.so.6       //This is here for tint2 to work.
~~~
go to his [repo](https://github.com/Axarva/dotfiles-2.0) it'll explain how to do it better
