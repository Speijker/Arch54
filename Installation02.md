###On a blank archlinux
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
