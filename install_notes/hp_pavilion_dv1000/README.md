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
### Configuring OpenBox
Openbox has 4 configuration files, all found in `~/.config/openbox`:
* `autostart` - This file is run whenever a new OpenBox session begins
* `environment`
* `menu.xml`
* `rc.xml`

### Setting a desktop background
* Install feh: `sudo apt install feh`
* feh is a lightweight image viewer that can set an X desktop background.
  You can set a desktop background with the follwing command:
  ```
  feh --bg-fill path/to/background/image.png
  ```
* feh can also set a random image from a directory. To have feh set a random
  background each time OpenBox starts, add the following to `autostart`:
  ```
  feh --randomize --bg-fill path/to/background/directory/*
  ```

### Audio setup
The audio on this system "worked" right out of the box, but volume could not
be controlled in any way. I was able to get this working after installing
ALSA.


