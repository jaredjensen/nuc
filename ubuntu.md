# Ubuntu

```bash
# 1. Create ~/.exrc so vi works correctly
set nocompatible
set backspace=2
# 2. Copy docker-compose.yml to home dir
# 3. Install dependencies
apt-get git
apt-get docker
# 4. Turn on Docker
docker-machine start default
# 5. Enable SSH
sudo apt update
sudo apt install openssh-server
sudo ufw allow 22
```

## Server Management

```bash
# Connect
ssh jared@192.168.86.208
# Shutdown
sudo poweroff
# Fix boot to initramfs
fsck /dev/mapper-nuc--vg-root (or whatever drive is reported)
```

## RAID

```bash
# List disks
sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL
# Create RAID
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1
# Make file system
sudo mkfs.ext4 /dev/md0
# Add RAID config
sudo chmod 777 /etc/mdadm/mdadm.conf
sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf
sudo chmod 644 /etc/mdadm/mdadm.conf
# Update initramfs
sudo update-initramfs -u
# Create mount point
sudo mkdir /mnt/raid
# Get UUID for raid
sudo blkid
# Add this to /etc/fstab
UUID=<uuid>    /mnt/raid    ext4    defaults    0    2
# Mount the RAID
sudo mount /dev/md0
# Confirm
df -h
```

## Samba

```bash
# Install Samba
sudo apt update
sudo apt install samba
# Open firewall
sudo ufw allow samba
# Create user and set password
sudo useradd raider
sudo passwd raider
# Set user’s Samba password
sudo smbpasswd -a raider
# Make this user the share mount owner
sudo chown -R raider /mnt/raid
# Add this /etc/samba/smb.conf
[raid1]
    comment = Mirror
    path = /mnt/raid
    writeable = yes
    read only = no
    browsable = yes
    public = yes
    create mask = 0644
    directory mask = 0755
    force user = raider
# Restart Samba
sudo service smbd restart
```
