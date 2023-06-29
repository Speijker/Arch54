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
Install nvidia drivers (GTX970: NV124(GM204) drivers, so nvidia package is appropriate, which includes nvidia.utils which disables opensource drivers from initram.

[Archwiki](https://wiki.archlinux.org/title/NVIDIA)
```
lspci -k | grep -A 2 -E "(VGA|3D)"
sudo pacman -S nvidia
sudo nano /etc/mkinitcpio.conf
>>> #remove kms from HOOKS array: prevent the initramfs from containing the nouveau module.
sudo mkinitcpio -P            #regenerate
```
Reboot. The nvidia-utils package contains a file which blacklists the nouveau module, so rebooting is necessary.

Once the driver has been installed, continue to #Xorg configuration or #Wayland.

[NVIDIA#Xorg_configuration](https://wiki.archlinux.org/title/NVIDIA#Xorg_configuration)

Wrong order, but it leads to Xorg installation above!

### Xorg install

Todo (but many things need to be done before this):

[plasma environment](itsfoss.com/install-kde-arch-linux/)    - This guy made some weird mistakes, but perhaps his instructions help clarify the vague arch-wiki plasma articles.
[Archwiki on KDE(plasma)](wiki.archlinux.org/title/KDE)      - Missing some details.
