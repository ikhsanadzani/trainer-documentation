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
pacstrap /mnt intel linux-lts linux-lts-headers iwd base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster
```
#fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
# chroot
```
arch-chroot /mnt
```
