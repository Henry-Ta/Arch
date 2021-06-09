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
$ pacman -Syy
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
$ mkfs.ext4 /dev/sda1		( format partition for root )
$ mkfs.ext4 /dev/sda4		( format partition for var  )
$ mkfs.ext4 /dev/sda5		( format partition for depot)
$ mkfs.ext4 /dev/sda6		( format partition for home )
$ mkfs.fat -F32 /dev/sda2 	( format efi for UEFI system)
$ mkswap /dev/sda3		    ( format partion for SWAP   )
($ mkfs.ext4 /dev/sda7		  format partition for boot )
```

## Mount Partitions
```
$ swapon /dev/sda3
$ mount /dev/sda1 /mnt
$ mkdir (/mnt/boot) /mnt/efi /mnt/home /mnt/var /mnt/depot 
$ mount /dev/sda2 /mnt/efi
$ mount /dev/sda4 /mnt/var
$ mount /dev/sda5 /mnt/depot
$ mount /dev/sda6 /mnt/home
```

## Start Arch Installation
```
$ pacstrap /mnt base base-devel linux/linux-lts linux-firmware neovim intel-ucode (amd-ucode)
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
$ hwclock --systohc 
```
```
$ nvim /etc/locale.gen
```
```
$ locale-gen
```
```
$ nvim /etc/locale.conf

LANG=en_US.UTF-8 	( or en_CA.UTF-8 )
```
```
$ nvim /etc/vconsole.conf

KEYMAP=us
```
```
$ nvim /etc/hostname

(arch-5547) arch-Y530
```
```
$ nvim /etc/hosts

127.0.0.1	localhost
::1		localhost
127.0.1.1	arch-5547.localdomain	arch-5547
```
```
$ pacman -S grub efibootmgr networkmanager network-manager-applet ntp dialog mtools dosfstools (linux-headers) linux-lts-headers bluez bluez-utils cups hplip gvfs openssh tlp alsa-utils pulseaudio pacman-contrib git xdg-utils xdg-user-dirs playerctl 
(bash-completion powertop )

```
```
$ (grub-install /dev/sda)
$ grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB 
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
