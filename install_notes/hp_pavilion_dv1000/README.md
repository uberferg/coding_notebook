# README.md
Installation notes for putting a minimal Debian install on an old HP Pavilion.

## OpenBox
Install Openbox: 
```
sudo apt install openbox obconf
```

To start an OpenBox session when you run `startx`, add the following line to `~/.xinitrc`:
```
exec /usr/bin/openbox-session
```

### Setting a desktop background
* Install feh: `sudo apt install feh`
* Use feh to set a background on `startx`:
