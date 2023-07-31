# How to configure and use both integrated and dedicated gpu's.

**Note:** I can only say about nvidia DGPU and intel IGPU as thats the only thing i have experience about. I'll link some website which explain that at the end.

## Step 1 - Configure both GPU's by following the steps mentioned in Nvidia.md and intel.md
1. if you want to manage the two GPU's manually i'll recommend [Denshi Wiki](https://wiki.denshi.org/hypha/client/nvidia). itll do a better way to explain things than me.
2. If you want a video tutorial - [NVIDIA Optimus: A ℂ𝕠𝕞𝕗𝕪 Guide](https://www.youtube.com/watch?v=Pn2iUgW3l6w&ab_channel=DenshiVideo)
3. Here im gonna talk about the easy setup that i use. [ENVY CONTROL](https://github.com/bayasdev/envycontrol)

## Step 2 - Install and use envycontrol
~~~
$ git clone https://github.com/geminis3/envycontrol.git
$ cd envycontrol
$ sudo pip install .
~~~
It's also available in the AUR
~~~
$ yay -S envycontrol
~~~
### Some examples

Set graphics mode to integrated:
~~~
sudo envycontrol -s integrated
~~~
Set graphics mode to hybrid and enable fine-grained power control:
~~~
sudo envycontrol -s hybrid --rtd3
~~~
Set current graphics mode to nvidia and specify to setup LightDM display manager
~~~
$ sudo envycontrol -s nvidia --dm lightdm
~~~
Revert all changes made by EnvyControl:
~~~
$ sudo envycontrol --reset
~~~
**Note:** If you are using dedicated gpu you'll have to go to hybrid first then to integrated.
## Go to bayasdev's [repo](https://github.com/bayasdev/envycontrol) to know the full information. You can also integrate the envycontrol as a GUI int gnome and KDE

## Wiki Articles
* [Hybrid graphics](https://wiki.archlinux.org/title/hybrid_graphics)
* [NVIDIA Optimus](https://wiki.archlinux.org/title/NVIDIA_Optimus)
