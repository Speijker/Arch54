# Arch54
Arch Linux installation walkthrough for myself

### Intro
A reminder to myself of the steps and installed packages when spooling a new ArchLinux installation.
Because I occasionally fuck up and have to re-install, and I keep forgetting what I did.

### Initial Install:
Start here: [https://wiki.archlinux.org/title/Installation_guide#top-page](Arch Linux Installation Guide).

List the keyboard layouts:
`ls /usr/share/kbd/keymaps/**/*.map.gz'
`loadkeys us-acentos' // closest to US-International with dead keys (for dutch ' and " usage

Verify Boot mode:
`ls /sys/firmware/efi/efivars' // should be filled with files
