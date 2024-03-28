
### Installing Lutris (06 2023)

install 32bit support for nvidia
```
sudo pacman -S lib32-nvidia-utils
```
There's a Lutris package in the Extra repository already:
```
sudo pacman -S lutris
```


Take not of the optional dependencies and install them as well:
```
sudo nano /etc/pacman.conf
  -- Uncomment the [multilib] section     #unable 32bit libraries
sudo pacman -Syu    #update repo
  -- reboot
```
```
sudo pacman -S gamemode gvfs innoextract lib32-gamemode lib32-vkd3d lib32-vulkan-icd-loader python-protobuf vkd3d vulkan-icd-loader vulkan-tools wine xorg-xgamma
```
Find Lutris in the applications in KDE-plasma.

###Installing Star Citizen?
Start Lutris: install lutris-GE-Proton8-8 x86_64 in it's wine version management. It took a long while to get going.

The LUG community has written a great script for installing, which is also uploaded (and hopefully maintained) on the AUR

get the LUG-helper script:
[AUR wiki](https://wiki.archlinux.org/title/Arch_User_Repository)
navigate to a build folder first (e.g. Downloads/builds)

This is the old script:
```
git clone https://aur.archlinux.org/lug-helper.git
makepkg -s -r -c    #optional options ##You may need to install base-devel meta-package for binaries!
sudo pacman -U lug-helper-1:2-7-1-any.pkg.tar.zst    #the name of your build package might differ
sudo pacman -S zenity    #required for the GUI
```
Launch LUG-Helper from your applications, and run through it. Additional notes below!

Instead of the old way above, use git clone https://github.com/starcitizen-lug/lug-helper.git (and us git pull to update). Run the lug-helper.sh  with `sh lug-helper.sh' in the correct folder.
It'll kick off without the user interface if you don't install zenity.


Their amazing github (with great instructions) resides here: https://github.com/starcitizen-lug/lug-helper

Also visit the org's discord: their #tech-desk has amazing people to help

### notes:
- ~~the install link in their LUG-helper script was broken: the button on Lutris.net works though!~~
    ^ This was fixed
- You may need to play with "prefer system libraries" in the wine runner options in Lutris (off for install, on to play, or vice versa).
