# Here i install pipewire. Tbh i like pipewire more than pulse audio as it has given me a lot less troubles than  pulse 

```
$ yay -S pipewire-jack pipwire-alsa pipewire-pulse qjackctl pavucontrol     // you can use paru too.
```

My Speakers didnt work after installing arch after installing these packages. Apparently i was missing "sof-firmware" . I got it after googling it and searching in ubuntu forums. My speakers worked after installing it. For more info on it you can go to [sof](https://www.sofproject.org/) 

Entering the command opens up a box with a lot of options. my favourite is the graph one. i can connect to different outputs by just dragging and i am able to output audio from both my heaphones and speakers from the same time. Its cool.  
```
$ qjackctl 
```
