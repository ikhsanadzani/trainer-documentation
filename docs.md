# partition
##lvm
```
pvcreate /dev/[partisi root]
```
```
vgcreate proc /dev/[partisi root]
```

## logical volume
```
lvcreate -L size (G | M) proc -n root
```
```
lvcreate -L size (G | M) proc -n vars
```
```
lvcreate -L size (G | M) proc -n vtmp
```
```
lvcreate -L size (G | M) proc -n vlog
```
```
lvcreate -L size (G | M) proc -n vaud
```
```
lvcreate -L size (G | M) proc -n home
```
```
lvcreate -L size (G | M) proc -n [name]
```

## luks
```
cryptsetup luksFormat /dev/proc/[name]
```

## formating 
```
mkfs.ext4 /dev/proc/root
```

```
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
```

```
mkfs.ext4 /dev/proc/vars
```

```
mkfs.ext4 /dev/proc/vtmp
```

```
mkfs.ext4 /dev/proc/vlog
```

```
mkfs.ext4 /dev/proc/vaud
```

```
mkfs.ext4 /dev/proc/home
```

```
mkfs.ext4 /dev/proc/[name]
```

## Mounting
```
mount /dev/proc/root /mnt
```

```
mount --mkdir -o uid=0,gid=0,dmask=0077,fmask=0077 /dev/paritition /mnt/boot
```

```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
```

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home
```
## packages
### intel
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware networkmanager lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster efibootmgr
```

### amd
```
pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware iwd lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster efibootmgr
```


## fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## formating tmpfs ke tmp
```
echo "/tmpfs /tmp  tmpfs  defaults,nosuid,nodev,noexec,size=1G  0  0" >> /mnt/etc/fstab
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
mkdir /home/user
```
```
useradd -d  /home/user [user name]
passwd [user name]
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
touch /etc/kernel/cmdline
```

```
echo "rd.luks.name=$(blkid -o UUID -s value /dev/patition_root)=proc root=/dev/proc/root" > /etc/kernel/cmdline
```

## secureboot

## BOOSTER
```
nvim /etc/booster.yaml
```
add value
```
network: true
modules: vfat, ext4, nvme
compression: zstd
enable_lvm: true
```
```
cd /boot
```
```
/usr/lib/booster/regenerate_images
```
```
booctl --path=/boot install
```
```
echo "title   Arch Linux Minimal" > /boot/loader/entries/arch.conf
```
```
echo "linux   /vmlinuz-linux-lts" >> /boot/loader/entries/arch.conf
```
```
echo "initrd  /intel-ucode.img" >> /boot/loader/entries/arch.conf
```
```
echo "initrd  /booster-linux-lts.img" >> /boot/loader/entries/arch.conf
```
```
echo "options $(cat /etc/kernel/cmdline) rw >> /boot/loader/entries/arch.conf
```
```
echo "default  arch.conf" >> /boot/loader/loader.conf
```
```
touch /etc/vconsole.conf
```

## booting
```
exit
```
```
umount -R /mnt
```
```
reboot
```
