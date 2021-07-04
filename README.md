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
$ mkswap /dev/sda1		    ( format partion for SWAP   )
$ mkfs.fat -F32 /dev/sda2 	( format efi for UEFI system)
$ mkfs.ext4 /dev/sda3		( format partition for root )
$ mkfs.ext4 /dev/sda4		( format partition for depot)
$ mkfs.ext4 /dev/sda5		( format partition for home )

($ mkfs.ext4 /dev/sda7		  format partition for boot )
```

## Mount Partitions

```
$ swapon /dev/sda1
$ mount /dev/sda3 /mnt
$ mkdir /mnt/home /mnt/depot /mnt/efi (/mnt/boot)
$ mount /dev/sda2 /mnt/efi
$ mount /dev/sda4 /mnt/depot
$ mount /dev/sda5 /mnt/home
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
$ echo "LANG=en_US.UTF-8" >> /etc/locale.conf
```

```
$ echo "KEYMAP=us" >> /etc/vconsole.conf
```

```
$ echo "arch-Y530" >> /etc/hostname
```

```
$ echo "127.0.0.1     localhost" >> /etc/hosts
$ echo "::1		localhost" >> /etc/hosts
$ echo "127.0.1.1     arch-Y530.localdomain	arch-Y530"      >> /etc/hosts
```

```
$ pacman -S grub efibootmgr (netctl dhcpcd dialog wpa_supplicant)/(networkmanager network-manager-applet) ntp dialog mtools dosfstools (linux-headers) linux-lts-headers bluez bluez-utils cups hplip gvfs openssh tlp tlp-rdw alsa-utils pulseaudio pacman-contrib git xdg-utils xdg-user-dirs playerctl
(bash-completion, powertop -> various powersaving modes )
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
$ systemctl enable ntpd
$ systemctl enable tlp

($ systemctl enable netctl ; systemctl enable dhcpcd ; systemctl enable wpa_supplicant)
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
