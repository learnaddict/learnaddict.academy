---
title: "Locally mounting a Raspberry Pi Raspbian Linux image"
date: "2017-09-16"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/raspberries.jpg"
  alt: "A close-up picture of some raspberries"
  relative: true
description: "Locally mounting a Raspberry Pi Raspbian Linux image"
---

It’s sometimes necessary to mount a Raspberry Pi Raspbian Linux image file before writing it to a SD card. This may be to configure an IP address, wireless settings, changing of passwords, etc.

If you are using your Raspberry Pis headless without a screen, this can be helpful for the first time startup. [Read more on configuring settings on Raspbian Linux images before writing them.](https://learnaddict.com/2016/02/23/modifying-a-raspberry-pi-raspbian-image-on-linux/)

This script may help with mounting a Raspbian image without having to do any calculations of where the partition start and finish. It is written for Ubuntu Linux.

```bash
#!/bin/bash
#
# Usage: sudo ./mount-raspbian-image
#

if [ -z "$1" ]
then
echo "Usage: sudo ./mount-raspbian-image "
exit
fi

IMG=$1

# Capture the patition details.
BOOT_PARTITION=`fdisk -l "$IMG" | grep "c W95 FAT32 (LBA)"`
ROOT_PARTITION=`fdisk -l "$IMG" | grep "83 Linux"`

# Grab the starting sector of the partitions.
BOOT_START_SECTOR=`echo "$BOOT_PARTITION" | awk '{print $2}'`
ROOT_START_SECTOR=`echo "$ROOT_PARTITION" | awk '{print $2}'`

# Calculate the start byte of the partitions.
((BOOT_START_BYTE=$BOOT_START_SECTOR * 512))
((ROOT_START_BYTE=$ROOT_START_SECTOR * 512))

# Grab the sector length of the partitions.
BOOT_SECTOR_LENGTH=`echo "$BOOT_PARTITION" | awk '{print $4}'`
ROOT_SECTOR_LENGTH=`echo "$ROOT_PARTITION" | awk '{print $4}'`

# Calculate the byte length of the partitions.
((BOOT_BYTE_LENGTH=$BOOT_SECTOR_LENGTH * 512))
((ROOT_BYTE_LENGTH=$ROOT_SECTOR_LENGTH * 512))

# Create the mount points.
sudo mkdir -p /mnt/image/boot
sudo mkdir -p /mnt/image/root

# Mount the partitions to the mount points.
mount -v -o offset=$BOOT_START_BYTE,sizelimit=$BOOT_BYTE_LENGTH -t vfat "$IMG" /mnt/image/boot
mount -v -o offset=$ROOT_START_BYTE,sizelimit=$ROOT_BYTE_LENGTH -t ext4 "$IMG" /mnt/image/root
```
