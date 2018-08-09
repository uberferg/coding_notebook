# Dell Latitude 7275 - Arch/Win10 Dual Boot



########################################
## Create a bootable ArchLinux USB drive 
Unless you have a USB C thumbdrive, you will need to buy an adapter in
order to use/boot the install medium.

To make the install disk, follow the ArchLinux guide for creating a bootable USB
[here](https://wiki.archlinux.org/index.php/USB_flash_installation_media).



###############################
## Partition the tablet's drive
If you wish to keep the initial Windows 10 installation, you should 
shrink and delete partitions in Windows using the official utility
(hit Win and type partition) to shrink and delete existing partitions.
I recommend keeping the EFI boot partition and the preexisting
recovery partition(s). The recovery partition(s) cannot be moved from
the official Windows utility, so if you want to do so use the 
[AOMEI Partition Assistant](https://www.disk-partition.com/free-partition-manager.html).

Creating partitions for Linux can be done from the Archlinux USB.
My partitions:
500M EFI System partition
128M Microsoft reserved partition
64G NTFS (Windows 10 installation)
remaining space, ~46G (data partition)
128G ext4 (Archlinux)



####################
## Boot into the USB
This will require some initial work from Windows 10. 

1. Navigate to the Recovery menu under Settings (hit Win and type boot) 
and click ‘Restart Now’ under ‘Advanced startup’.

2. When the PC restarts, select ‘UEFI Firmware Settings’, which will 
bring you to the UEFI setup screen.

3. Under ‘Secure Boot/Secure Boot Enable’, disable Secure Boot
4. Under General/Advanced Boot Options’, check the boxes for
    - Enable Legacy Option ROMs
    - Enable UEFI Network Stack
5. Under ‘System Configuration/USB/Thunderbolt Configuration’, 
check the box for ‘Enable Thunderbolt Boot Support’
6. *OPTIONAL* Under ‘POST Behavior/Extend BIOS POST Time’, set the option 
to something other than 0 seconds to make it easier to access the F2/F12 
menus on device boot.

Now reboot with the install USB plugged in. Hold F12 to enter the boot menu 
and select your USB from the list. This should boot into the Archlinux command
line so you can continue with the Arch installation.



#####################
## Arch Install guide

### Verify UEFI boot mode:
    # ls /sys/firmware/efi/efivars

If the directory does not exist, the system may be in BIOS mode


### Connect to the Internet
This must be done wirelessly since the tablet does not have an ethernet port.
The connection can be verified with `ping`.

1. Check if the driver for the wireless card has been loaded. Look for
an entry labeled "Network Controller" and you should see something next
to "Kernel driver in use".
    # lspci -k

2. Find the name of the wireless interface (mine is wlp108s0):
    # ip link

3. Bring the interface down, if needed:
    # ip link set <interface> down
To verify that the interface is down, inspect the output of:
    # ip link show <interface>
The `UP` in `<BROADCAST,MULTICAST,UP>` is what indicates the
interface is up, not the later `state DOWN`.

4. Create a `netctl` profile:
    # wifi-menu -o

5. Start the profile with
    # netctl start <profile-name>
and check the status of the profile with
    # netctl status <profile-name>


### Update the system clock
    # timedatectl set-ntp true
To check the service status, use `timedatectl status`.


### Partition and format the disk
You will need a partition for the root directory and an EFI system partition.
These can be created with `cfdisk`, `fdisk`, and `parted`, all included
on the Arch USB.

If keeping the original Windows 10 installation, you can reuse the existing
EFI partition.

Once the root partition (and any other partitions) have been created, 
format them with
    # mkfs.ext4 /dev/sdx#

For swap partitions, initialize them with
    # mkswap /dev/sdx#
    # swapon /dev/sdx#


### Mount the file systems
Existing partitions can be listed with `lsblk`.

Mount the root partition on to `/mnt`:
    # mount /dev/sdx1 /mnt

Create mount points for any remaining paritions and mount them accordingly:
    # mkdir /mnt/boot
    # mount /dev/sdx2 /mnt/boot


### Installation
Edit the mirrorlist at `/etc/pacman.d/mirrorlist`, moving the desired
mirrors to the top of the file.

Use `pacstrap` to install the `base` package group:
    # pacstrap /mnt base

Additional packages and groups can be installed by appending the names
to `pacstrap`. Installing the dependencies for `wifi-menu` is recommended,
especially if it was used to set up the network earlier in the installation.
    # pacstrap /mnt base base-devel wpa_supplicant dialog vim git tmux

If reinstalling Linux without reformatting/repartitioning the EFI
partition, the old Linux boot files may have to be manually removed
from `/mnt/boot` for pacstrap to succeed.

Generate an fstab file (use `-U` or `-L` to define by UUIDs or labels.
    # genfstab -U /mnt >> /mnt/etc/fstab

Check the resulting file in `/mnt/etc/fstab` and edit in case of errors.

Lastly, change root into the new system
    # arch-chroot /mnt


### Configuration
Set the time zone and the system clock:
    # ln -sf /usr/share/zoneinfo/<Region>/<City> /etc/localtime
    # hwclock --systohc

Uncomment `en_US.UTF-8` and other needed locales in `/etc/locale.gen`
and generate them with
    # locale-gen
then set the `LANG` variable in `/etc/locale.conf` accordingly:
    LANG=en_US.UTF-8

Create the hostname file at `/etc/hostname`:
    myhostname
Then add matching entries to `/etc/hosts`:
    127.0.0.1   localhost
    ::1         localhost
    127.0.1.1   myhostname.localdomain  myhostname

Create a new initramfs, if needed
    # mkinitcpio -p linux

Set the root password:
    # passwd


### Boot loader installation
Lastly, we must install a Linux-capable bootloader, such as GRUB or
systemd-boot.

If you have an Intel CPU (this tablet should), also install the
`intel-ucode` package so you can enable microcode updates in the next step.
    # pacman -S intel-ucode


##### systemd-boot
(below, esp denotes the EFI system partition mountpoint, typically `/boot`)
1. Install systemd-boot to the EFI system partition:
    # bootctl --path=esp install

2. Add a loader file at `esp/loader/entries/arch.conf`:
    title   Arch Linux
    linux   /vmlinuz-linux
    initrd  /intel-ucode.img
    initrd  /initramfs-linux.img
    options root=UUID=uuid-goes-here rw
The third line enables the microcode updates mentioned above.
Windows does not need to be manually configured, bootctl will
check for it automatically at boot time.

3. It is recommended to install `systemd-boot-pacman-hook` from the AUR
to automate the process of updating bootctl.


### Reboot
Exit the chroot with `exit` or by pressing `Ctrl+D`.

Optionally manually unmount all the partitions with `umount -R /mnt`.

Lastly, `reboot`.





## Post-Install
## Display Setup
## Xorg
## Awesome
## Audio Setup
Install alsa-utils. Use amixer or alsa-mixer to unmute Master and PCM channels
## Mouse & Keyboard Setup


