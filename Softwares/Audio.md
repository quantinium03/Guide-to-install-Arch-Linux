# Here i install pipewire. Tbh i like pipewire more than pulse audio as it has given me a lot less troubles than  pulse 

```
$ sudo pacman -S pipewire-jack pipwire-alsa pipewire-pulse qjackctl pavucontrol
```

My Speakers didnt work after installing arch after installing these packages. Apparently i was missing "sof-firmware" . I got it after googling it and searching in ubuntu forums. My speakers worked after installing it. For more info on it you can go to [sof](https://www.sofproject.org/) 
