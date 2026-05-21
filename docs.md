# partition
```
cryptsetup luksFormat /dev/partition
```
```
cryptsetup luksOpen /dev/partition system
```
```
pvcreate /dev/mapper/system
```
```
vgcreate proc /dev/mapper/system
```

## root
```
lvcreate -L size (G | M) proc -n root
```
```
mkfs.ext4 /dev/proc/root
```
```
mount /dev/proc/root /mnt
```

## boot
```
mkfs.vfat -F32 -n BOOT /dev/partition
```
```
mkdir /mnt/boot
```
```
mount /dev/paritition /mnt/boot
```


## var
```
lvcreate -L size (G | M) proc -n vars
```
```
mkfs.ext4 /dev/proc/vars
```
```
mkdir /mnt/var
```
```
mount -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```


## vtmp
```
lvcreate -L size (G | M) proc -n vtmp
```
```
mkfs.ext4 /dev/proc/vtmp
```
```
mkdir /mnt/var/tmp
```
```
mount -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
```

## vlog
```
lvcreate -L size (G | M) proc -n vlog
```
```
mkfs.ext4 /dev/proc/vlog
```
```
mkdir /mnt/var/log
```
```
mount -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```

## vaud
```
lvcreate -L size (G | M) proc -n vaud
```
```
mkfs.ext4 /dev/proc/vaud
```
```
mkdir /mnt/var/log/audit
```
```
mount -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```

## temp
```
lvcreate -L size (G | M) proc -n temp
```
```
mkfs.ext4 /dev/proc/temp
```
```
mkdir /mnt/tmp
```
```
mount -o rw,nodev,nosuid,noexec,relatime /dev/proc/temp /mnt/tmp
```

## home
```
lvcreate -L size (G | M) proc -n home
```
```
mkfs.ext4 /dev/proc/home
```
```
mkdir /mnt/home
```
```
mount -o rw,nodev,nosuid,relatime /dev/proc/home /mnt/home
```
# packages
```
pacstrap /mnt intel linux-lts linux-lts-headers iwd lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster
```
# fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
# network
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
```
mkdir /mnt/var/lib/iwd
```
```
cp -r /var/lib/iwd/* /mnt/var/lib/iwd
```
# chroot
```
arch-chroot /mnt
```



## jika 1 kata tidak perlu pake `""` kalo lebih menggunakan petik `""`
```
echo [nama komputer] > /etc/hostname
```

## LOCALTIME
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
****
## LOCALE

```
nvim /etc/locale.gen
```

## lalu pencarian di nvim menggunakan `/`

```
lalu uncommenting kedua en_US
```

### generate bahasa yg di uncommenting 
```
locale-gen
```

```
locale > /etc/locale.conf
```

### config locale
```
nvim /etc/locale.conf
```
### config file locale 
```
isi lang=C menjadi lang=en_US.UTF-8
dan isi ALL=en_US.UTF-8
```
****
## USERADD
```
useradd -m [user]
passwd [user]
```
```
echo 'nama_user ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none
```
```
usermod -aG wheel [user]
```
****

## KERNEL PARAMETER
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-mods.conf,03-secs.conf,04-perf.conf,05-misc.conf}
```

## CONFIG KERNEL PARAMETER

### 01-boot.conf
```
echo "rd.luks.name=$(blkid -o UUID -s value /dev/patition_root)=proc root=/dev/proc/root" > /etc/cmdline.d/01-boot.conf
```
### 05-misc.conf
```
nvim /etc/cmdline.d/05-misc.conf
```
```
rw quiet
```
## PREPARE BOOT
```
mkdir /boot/kernel /boot/efi /boot/efi/linux
```
### Jika intel
```
mv /boot/intel-ucode kernel/
```

```
mv /boot/vmlinuz-linux-lts /boot/intel-ucode.img kernel/
```
### Jika amd
```
mv /boot/amd-ucode kernel/
```

```
mv /boot/vmlinuz-linux-lts /boot/amd-ucode.img kernel/
```

## MKINITCPIO
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf
```
change section hooks like this
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)
```
```
nvim /etc/mkinitcpio.d/linux.preset
```

```
# mkinitcpio preset file for the 'linux-lts' package

ALL_config="/etc/mkinitcpio.d/default.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-lts"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-lts"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#efault_image="/boot/initramfs-linux-lts.img"
default_uki="/boot/efi/linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-lts-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect"
```
