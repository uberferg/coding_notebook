# README.md
Installation notes for putting a minimal Debian install on an old HP Pavilion.

# System Info
* `lspci` output:
  ```
  00:00.0 Host bridge: Intel Corporation Mobile 945GM/PM/GMS, 943/940GML and 945GT Express Memory Controller Hub (rev 03)
  00:02.0 VGA compatible controller: Intel Corporation Mobile 945GM/GMS, 943/940GML Express Integrated Graphics Controller (rev 03)
  00:02.1 Display controller: Intel Corporation Mobile 945GM/GMS/GME, 943/940GML Express Integrated Graphics Controller (rev 03)
  00:1b.0 Audio device: Intel Corporation NM10/ICH7 Family High Definition Audio Controller (rev 02)
  00:1c.0 PCI bridge: Intel Corporation NM10/ICH7 Family PCI Express Port 1 (rev 02)
  00:1c.1 PCI bridge: Intel Corporation NM10/ICH7 Family PCI Express Port 2 (rev 02)
  00:1d.0 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #1 (rev 02)
  00:1d.1 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #2 (rev 02)
  00:1d.2 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #3 (rev 02)
  00:1d.3 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #4 (rev 02)
  00:1d.7 USB controller: Intel Corporation NM10/ICH7 Family USB2 EHCI Controller (rev 02)
  00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev e2)
  00:1f.0 ISA bridge: Intel Corporation 82801GBM (ICH7-M) LPC Interface Bridge (rev 02)
  00:1f.1 IDE interface: Intel Corporation 82801G (ICH7 Family) IDE Controller (rev 02)
  00:1f.2 SATA controller: Intel Corporation 82801GBM/GHM (ICH7-M Family) SATA Controller [AHCI mode] (rev 02)
  00:1f.3 SMBus: Intel Corporation NM10/ICH7 Family SMBus Controller (rev 02)
  02:00.0 Network controller: Intel Corporation PRO/Wireless 3945ABG [Golan] Network Connection (rev 02)
  05:08.0 Ethernet controller: Intel Corporation PRO/100 VE Network Connection (rev 02)
  05:09.0 FireWire (IEEE 1394): Ricoh Co Ltd R5C832 IEEE 1394 Controller
  05:09.1 SD Host controller: Ricoh Co Ltd R5C822 SD/SDIO/MMC/MS/MSPro Host Adapter (rev 19)
  05:09.2 System peripheral: Ricoh Co Ltd R5C592 Memory Stick Bus Host Adapter (rev 0a)
  05:09.3 System peripheral: Ricoh Co Ltd xD-Picture Card Controller (rev 05)
  ```


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

