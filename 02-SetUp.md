## set-up
### initiate and settle user

On a blank archlinux
We can continue here: [General Recommend](https://wiki.archlinux.org/title/General_recommendations)

Remember the optional installed packages at the pacstrap step previously (like sudo, nano and vim)

Make a user, and add to the wheel group to grant sudo/admin privilege (do not grant sudo to user directly, but put in wheel group!)
```
useradd -m speijker         #add your own username!
passwd speijker
chfn speijker               #fun
usermod -aG wheel speijker  #add to the admin user group
ln -s /usr/bin/vim /usr/bin/vi #so visudo can find the VIM editor
visudo /etc/sudoers
>>> uncomment %wheel ALL=(ALL:ALL) ALL
    press esc, :w to write, :q to quit (or :wq)
```
install and enable reflector for package mirrors (default ones are sometimes 404)
```
sudo pacman -S reflector
sudo nano /etc/xdg/reflector/reflector.conf            #uncomment and add Netherlands for use in reflector.service
sudo systemctl enable reflector.timer                  #rather than .service, this runs once a week by default.
```

[Archwiki](https://wiki.archlinux.org/title/NVIDIA): Install nvidia drivers (GTX970: NV124(GM204) drivers, so nvidia package is appropriate, which includes nvidia.utils which disables opensource drivers from initram.

Note: you may need the multilib library here already: see part 03-Lutris.md, section 1

```
lspci -k | grep -A 2 -E "(VGA|3D)"
sudo pacman -S nvidia
sudo nano /etc/mkinitcpio.conf
>>> #remove kms from HOOKS array: prevent the initramfs from containing the nouveau module.
sudo mkinitcpio -P            # regenerate
sudo reboot                   # The nvidia-utils package contains a file which blacklists the nouveau module, so rebooting is necessary.
```
Once the driver has been installed, continue to #Xorg configuration or #Wayland.

### Xorg install
[Xorg](https://wiki.archlinux.org/title/Xorg). One might try Wayland instead (newest drivers!) but that's for another time.

Install the group (xorg-server package is also fine, but the xorg group also contains items form the xorg-apps package which might be needed:
```
sudo pacman -S xorg
```
 Arch supplies default configuration files in /usr/share/X11/xorg.conf.d/, and no extra configuration is necessary for most setups. Still...

 [NVIDIA Xorg Config](https://wiki.archlinux.org/title/NVIDIA#Xorg_configuration}
```
sudo nvidia-xconfig            #automatic nvidia configuration for xorg.
sudo nano /etc/X11/xorg.conf   #doublecheck resolution etc.
```

### KDE Plasma install
[Arch KDE](https://wiki.archlinux.org/title/KDE#Plasma) (and check [this (not all: bad user management!](https://itsfoss.com/install-kde-arch-linux/)
```
sudo pacman -S plasma plasma-wayland-session
```
enable drm (since we are using the proprietary driver and not open source)
```
sudo nano /etc/default/grub
>> add  nvidia_drm.modeset=1 kernel parameter (append between the quotes of GRUB_CMDLINE_LINUX_DEFAULT).
sudo grub-mkconfig -o /boot/grub/grub.cfg            #regenerate grub
```
```
sudo pacman -S kde-applications
```
Optional network manager:
```
=following two lines: dhcpcd will generally be fine for wired network only: it's bloated, but auto-config of network-connection is better for e.g. Star Citizen
sudo pacman -S networkmanager
sudo systemctl enable NetworkManager.service
```
Display Manager (SDDM is reccommend for KDE plasma)
```
sudo pacman -S sddm        #The sddm-kcm package (included in the plasma group) provides a GUI to configure SDDM in Plasma's system settings
sudo systemctl enable sddm.service
```
Automate initcpio update after nvidia drivers are updated!
```
sudo nano /etc/pacman.d/hooks/nvidia.hook
```
Create the following in the above:
```
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
Target=nvidia
Target=linux
# Change the linux part above and in the Exec line if a different kernel is used

[Action]
Description=Update NVIDIA module in initcpio
Depends=mkinitcpio
When=PostTransaction
NeedsTargets
Exec=/bin/sh -c 'while read -r trg; do case $trg in linux) exit 0; esac; done; /usr/bin/mkinitcpio -P'
```

I hope that's it: if it boots, we'll continue with it's [configuration](https://wiki.archlinux.org/title/KDE#Configuration) (which is done inside plasma).
```
sudo reboot
```
