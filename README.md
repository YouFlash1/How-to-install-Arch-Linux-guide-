# How-to-install-Arch-Linux-guide-
How to install Arch Linux (guide)

## Description:
### Load your keymap  
**Commande:**
```sh
loadkeys fr
# or
loadkeys en
```

---

### Fix the font if needed  
**Commande:**

**check all the fonts available**
```sh
ls /usr/share/kbd/consolefont
```

**change the font**
```sh
setfont ter-224b.psf.gz
```

---

### Connection Wifi  
**Commande:**
```sh
systemctl start iwd
iwctl
station list
station wlan0 scan
station wlan0 get-networks
station wlan0 connect <wifi name>
# Passphrase: <wifi password>
exit
```
verify the wifi connection:
```sh
ping https://youflash1.wuaze.com
```

---

### Set the time  
**Commande:**
```sh
timedatectl set-ntp true
timedatectl status
```

---

### Setup the disk (partitions)
**Commande:**
```sh
cfdisk
```
select the label, depending on you but I use “gpt”

```sh
# then ably the following separation of partitions:
# sda1 - 1G - bootloader
# sda2 - 4G - swap_partition
# sda3 - rest - root_partition
```

**partitions formating :**
```sh
mkfs.fat -F 32 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3
```

**partitions mounting:**
```sh
mount /dev/sda3 /mnt
mkdir -p /mnt/efi
mount /dev/sda1 /mnt/efi
swapon /dev/sda2
```

**verify the good disk:**
```sh
lsblk
```

---

### Install Arch Linux base system  
**Commande:**
```sh
pacstrap /mnt base linux linux-firmware
genfstab -U /mnt >> /mnt/etc/fstab
```

---

### Continue in chroot  
**Commande:**
```sh
arch-chroot /mnt
ls
```

---

### Configure the new system  
**Commands:**

#### Time
```sh
ln -sf /usr/share/zoneinfo/<your timezone> /etc/localtime
hwclock --systohc
```

#### Pacman & Locale
```sh
pacman -Syu vim
vim /etc/locale.gen
# Uncomment: en_US.UTF-8 UTF-8
```

#### Hostname
```sh
vim /etc/hostname
# add your hostname
# then exit (Esc) + (:x)
```

#### Root password
```sh
passwd
# Enter the host password
```

#### Hosts file
```sh
vim /etc/hosts
# See hosts(5) for details.
# 127.0.0.1       localhost
# ::1             localhost
# 127.0.1.1       <hostname>.localdomain <hostname>
```

#### Add new user
```sh
useradd -G wheel,audio,video -m <new user>
passwd <new user>
# Enter the new user password
```

#### Network and utilities
```sh
pacman -Syu netctl 
pacman -Syu dhcpcd 
pacman -Syu networkmanager 
pacman -Syu network-manager-applet 
pacman -Syu dialog 
pacman -Syu wpa_supplicant 
pacman -Syu sudo
```

#### Enable sudo for wheel
```sh
EDITOR=vim visudo
# Uncomment: %wheel ALL=(ALL) ALL
```

#### Install and configure grub
```sh
pacman -Syu grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/efi/--bootloader-id=Arch
grub-mkconfig -o /boot/grub/grub.cfg
exit
reboot
```

> Remember to remove the USB installer before reboot.

---

### Wifi connection again after instalation + install Hyprland  
**Commande:**
```sh
sudo systemctl enable systemd-networkd
sudo systemctl start systemd-networkd
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved
ip addr
sudo nano /etc/systemd/network/20-wired.network
```

---

### Boot PC and install Hyprland UI  
**Commande:**
```sh
timedatectl set-ntp true
```

---

### Notes  
- Clear terminal: `Ctrl + L`  
- Exit vim:
  ```sh
  Esc
  :x
  Enter
  ```
  `:x` = quit & save  
  `:wq` = quit & save  
  `:q!` = quit without save
"""
