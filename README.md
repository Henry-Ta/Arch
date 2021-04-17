# Arch Installation

## Configures Wifi
```
$ rfkill unblock all
$ iwctl
$ device list 		( chose wlan0 or any wifi network )
$ station wlan0 scan
$ station wlan0 get-networks
$ station wlan0 connect "wifi1"
$ exit
```
## Update the system clock
```
$ timedatectl set-ntp true
$ timedatectl status
```

## Disk Partition
```
$ lsblk
$ fdisk -l
$ cfdisk /dev/sda 
```

## Formats Partitions
```
$ mkfs.ext4 /dev/sda4		( format partition for root )
$ mkfs.ext4 /dev/sda1		( format partition for home )
$ mkfs.ext4 /dev/sda7		( format partition for boot )
$ mkswap /dev/sda2		( format partion for SWAP )
$(mkfs.fat -F32 /dev/sda3 	  format boot/efi for UEFI system)
```

## Mount Partitions
```
$ swapon /dev/sda2
$ mount /dev/sda4 /mnt
$ mkdir -p /mnt/boot (/mnt/boot/efi) /mnt/home /mnt/disk1 /mnt/disk2 
$ mount /dev/sda1 /mnt/home
$ mount /dev/sda7 /mnt/boot
$ mount /dev/sda5 /mnt/disk1
$ mount /dev/sda6 /mnt/disk2
```

## Start Arch Installation
```
$ pacstrap /mnt base linux(linux-lts) linx-firmware neovim intel-ucode (amd-ucode)
```
```
$ genfstab -U /mnt >> /mnt/etc/fstab
```
```
$ arch-chroot /mnt
```
```
$ ln -sf /usr/share/zoneinfo/America/Vancouver /etc/localtime
```
```
$ hwclock --systohc -u
```
```
$ nvim /etc/locale.gen
```
```
$ locale-gen
```
```
$ nvim /etc/locale.conf

LANG=en_CA.UTF-8 	( or en_US.UTF-8 )
```
```
$ nvim /etc/vconsole.conf

KEYMAP=us
```
```
$ nvim /etc/hostname

arch-5547 (arch-Y530)
```
```
$ nvim /etc/hosts

127.0.0.1	localhost
::1		localhost
127.0.1.1	arch-5547.localdomain	arch-5547
```
```
$ pacman -S grub efibootmgr networkmanager network-manager-applet wpa_supplicant wireless_tools netctl ntp dialog mtools dosfstools base-devel linux-headers (linux-lts-headers) bash-completion bluez bluez-utils cups hplip gvfs openssh tlp powertop alsa-utils pulseaudio pulseaudio-bluetooth git (reflector) xdg-utils xdg-user-dirs

```
```
$ grub-install /dev/sda
$ (grub-install --target=x86_64-efi bootloader-id=GRUB --efi-directory=/boot/efi)
$ grub-mkconfig -o /boot/grub/grub.cfg
```
```
$ systemctl enable NetworkManager
$ systemctl enable bluetooth
$ systemctl enable cups
$ systemctl enable sshd
$ systemctl enable tlp
$ systemctl enable ntpd

```

```
($ nvim /etc/xdg/reflector/reflector.conf)
($ systemctl enable reflector)
```
```
$ passwd
$ useradd -m -g users -G wheel henry
$ passwd henry
$ nvim /etc/sudoers
```
```
$ exit
$ umount -a
$ reboot
```
