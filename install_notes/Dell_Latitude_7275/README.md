

## Create a bootable ArchLinux USB drive 
The official Arch guide should work without issues: https://wiki.archlinux.org/index.php/USB_flash_installation_media

## Partition the system drive. 
If you wish to keep the initial Windows 10 installation, you should shrink and delete partitions in Windows using the official utility (hit Win and type partition) to shrink and delete existing partitions.
Creating partitions for Linux can be done from the Archlinux USB.
My partitions:
500M EFI System partition
128M Microsoft reserved partition
64G NTFS (Windows 10 installation)
remaining space, ~46G (data partition)
128G ext4 (Archlinux)

## Boot into the USB
This will require some initial work from Windows 10. 
Navigate to the Recovery menu under Settings (hit Win and type boot) and click ‘Restart Now’ under ‘Advanced startup’.
When the PC restarts, select ‘UEFI Firmware Settings’, which will bring you to the UEFI setup screen.
Under ‘Secure Boot/Secure Boot Enable’, disable Secure Boot
Under General/Advanced Boot Options’, check the boxes for
Enable Legacy Option ROMs
Enable UEFI Network Stack
Under ‘System Configuration/USB/Thunderbolt Configuration’, check the box for ‘Enable Thunderbolt Boot Support’
[OPTIONAL] Under ‘POST Behavior/Extend BIOS POST Time’, set the option to something other than 0 seconds to make it easier to access the F2/F12 menus on device boot.
Now reboot with the install USB plugged in. Hold F12 to enter the boot menu and select your USB from the list. This should boot into the Archlinux command line so you can continue with a typical Arch installation.

## Arch Install guide:
Verify boot mode. If the directory does not exist, the system may be in BIOS mode
`# ls /sys/firmware/efi/efivars`
Connect to the Internet. This needs to be done wirelessly since the 7275 does not have an ethernet port
Find the name of the wireless interface (mine is wlp108s0):
`# ip link`
Scan for available access points. Networks with an RSN information block connect with WPA2:
`# iw dev interface scan | less`
Connect to an access point:
`# wpa_supplicant -B -i interface  -c <(wpa_passphrase “your_SSID” “your_key”)`
-B option runs in the background. Do this after confirming the connection will succeed.
Output of wpa_passphrase can be saved to a config file to use later:
`# wpa_passphrase “your_SSID” “your_key” > /etc/wpa_supplicant/my_config.conf`




Post-Install
Display Setup
Xorg
Awesome
Audio Setup
 Install alsa-utils
Use amixer or alsa-mixer to unmute Master and PCM channels
Mouse & Keyboard Setup


