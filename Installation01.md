# Arch54
Arch Linux installation walkthrough for myself

### Intro
A reminder to myself of the steps and installed packages when spooling a new ArchLinux installation.

Because I occasionally fuck up and have to re-install, and I keep forgetting what I did.

### Initial Install:
Start here: [Arch Linux Installation Guide](https://wiki.archlinux.org/title/Installation_guide#top-page).

List and set the keyboard layouts: closest to US-International with dead keys (for dutch ' and " usage
```
ls /usr/share/kbd/keymaps/**/*.map.gz
loadkeys us-acentos
```
Verify Boot mode: should be filled with files
```
ls /sys/firmware/efi/efivars
```
The damn clock:
```
timedatectl set-timezone Europe/Amsterdam
timedatectl
```
Partitioning drives (one bootloader, 1GB; one SWAP, 30G (though equal to RAM is usually fine unless playing star Citizen). Rest is /root.
```
fdisk -l        #list devices
fdisk /dev/sda  #in my case the SSD
IN FDISK PROMPT:
>> g            #new GPT partition table
>> n            #new partition table
>> 1            #label (1, 2 and 3)
>> 2048         #starting point
>> +1GB         #endpoint is +1 G
>> t            #change type (1=EFI, 19 = SWAP, 20=LINUX
>> 1
>> 1#change to efi (for boot part)
>> n            #new
>> 2
>> default
>> +30G
>> t
>> 2
>> 19          #designated SWAP
>> n
>> n
>> 3
>> default
>> default    #should create Linux Filesystem with whatever''s left of the disk.
>> w          #write all to disk!
```
Formatting the disk and mounting
```
mkfs.ext4 /dev/sda3  #write rootfolder
mkswap /dev/sda2
mkfs.fat -F 32 /dev/sda1
mount /dev/sda3 /mnt
mount --mkdir /dev/sda1 /mnt/boot
swapon /dev/sda2
```
Installing the base packages
```
pacstrap -K /mnt base linux linux-firmware
```
Install additional packages?
```
pacstrap -K /mnt filesystem vim nano sudo intel-ucode dhcpcd
```
generate fstab file (disk index, by label for me:)
```
genfstab -L /mnt >> /mnt/etc/fstab
nano /mnt/etc/fstab                     # check all is correctly listed.
```

To get wired network, I found I had to:

create the file /etc/systemd/network/20-wired.network

[Match]
Name=en*
Name=eth*

[Network]
DHCP=yes
Then either restart or start:

$ sudo systemctl start systemd-networkd
$ sudo systemctl start systemd-resolved
It's a lot easier if you remember to install dhcpcd, and sudo systemctl start dhcpcd. I've forgotten on a few test systems.

I found parsing the wiki for the info to get a functional network using systemd somewhat obscured and time consuming? But I'm just a stupid ape that likes Arch! YMMV
