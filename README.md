# Arch Installation

## Step 1: Configures Wifi
```
$ iwctl
$ device list 		( chose wlan0 or any wifi network )
$ station wlan0 scan
$ station wlan0 get-networks
$ station wlan0 connect "wifi1"
$ exit
```

## Step 2: Disk Partition
```
$ lsblk
$ fdisk -l
$ cfdisk /dev/sda 
```

## Step 3: Formats Partitions
```
$ mkfs.ext4 /dev/sda4		( format partition for root )
$ mkfs.ext4 /dev/sda1		( format partition for home )
$ mkfs.ext4 /dev/sda7		( format partition for boot )
$ mkswap /dev/sda2		( format partion for SWAP )
```

## Step 4: Mount Partitions
```
$ swapon /dev/sda2
$ mount /dev/sda4 /mnt
$ mkdir /mnt/boot /mnt/home /mnt/disk1 /mnt/disk2
$ mount /dev/sda1 /mnt/home
$ mount /dev/sda7 /mnt/boot
$ mount /dev/sda5 /mnt/disk1
$ mount /dev/sda6 /mnt/disk2
```

## Step 5 >: Start Arch Installation
```
$ pacstrap /mnt base linux-lts linx-firmware neovim intel-ucode amd-ucode
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

LANG=en_CA.UTF-8 	( or en_US.UTF-8 )
```
```
$ nvim /etc/vconsole.conf

KEYMAP=us
```
```
$ nvim /etc/hostname

arch-5547
```
```
$ nvim /etc/hosts

127.0.0.1	localhost
::1		localhost
127.0.1.1	arch-5547.localdomain	arch-5547
```
```
$ pacman -S grub networkmanager network-manager-applet dialog mtools dosfstools base-devel linux-lts-headers bluez bluez-utils cups alsa-utils pulseaudio pulseaudio-bluetooth git reflector xdg-utils xdg-user-dirs
```
```
$ grub-install /dev/sda
$ grub-mkconfig -o /boot/grub/grub.cfg
```
```
$ systemctl enable NetworkManager
$ systemctl enable bluetooth
$ systemctl enable cups
```
```
$ passwd
$ useradd -m henry
$ passwd henry
$ nvim /etc/sudoers
```
```
$ exit
$ umount -a
$ reboot
```
