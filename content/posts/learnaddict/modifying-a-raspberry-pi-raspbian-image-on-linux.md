---
title: "Modifying a Raspberry Pi Raspbian Image on Linux"
date: "2016-02-23"
author: "Matt Saunders"
description: "Modifying a Raspberry Pi Raspbian Image on Linux"
---

If you have lots of Raspberry Pi computers, it’s useful to have an image that is preconfigured with your WiFi connections details, default password, default IP address, etc.

I started by downloading the latest Raspian image from the Raspberry Pi Foundation Website. I am using the smaller Lite version, but will work for the full desktop image as well.

After downloading the Raspbian image, ensure the checksum of the file matches the associated sha1 checksum from the downloads page. To calculate the checksum, use the command `sha1sum 20xx-xx-xx-raspbian-jessie-lite.zip`. Please change 20xx-xx-xx-raspbian-jessie-lite.zip to match the name of the image you download.

If the SHA1 checksum doesn’t match, it has either been corrupted or tampered with. If the image is fine, it now needs to be uncompressed. The unzip command can do this for you, using `unzip 20xx-xx-xx-raspbian-jessie-lite.img`.

In order to change the file system inside the image, we need to mount the partitions inside the image file. This is relatively simple but will need some maths calculations to be done.

Running the command `sudo fdisk -l 20xx-xx-xx-raspbian-jessie-lite.img` will show the partitions that are configured inside the image file. The important columns are the Start Sectors and Type. The /boot partition will have the type of c W95 FAT32 (LBA) and is where the system boot configuration is stored, and the /root partition will have the type of 83 Linux.

```
Disk 20xx-xx-xx-raspbian-jessie-lite.img: 1.4 GiB, 1458569216 bytes, 2848768 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x34e4b02c

Device Boot Start End Sectors Size Id Type
20xx-xx-xx-raspbian-jessie-lite.img1 8192 131071 122880 60M c W95 FAT32 (LBA)
20xx-xx-xx-raspbian-jessie-lite.img2 131072 2848767 2717696 1.3G 83 Linux
```

The output may be different for your image. Using your output, let’s do some simple calculations. It may be useful to note these numbers down as you go.

In order to mount the partitions, we need to identify where the partition starts within the image. The above output provides us the Start sector location, but the mount command uses the number of bytes from the beginning of the file.

The calculation is Start Sector * 512. My /boot partition calculation is 8192 * 512 = 4194304. My /root partition calculation is131072 * 512 = 67108864. These numbers will vary depending on which image and release date you have downloaded.

The partitions will be mounted to my local file system, mounting the /boot partition to /mnt/image/boot and the /root partition to /mnt/image/root. These folders will need to be created using the following commands.

```bash
sudo mkdir -p /mnt/image/boot
sudo mkdir -p /mnt/image/root
```

The partitions can now be mounted on these folders with the following commands. The value afteroffset= is the value you calculated using the above calculation for that partition. The values here are correct for my image but may differ from the values you calculated.

```bash
sudo mount -v -o offset=4194304 -t vfat 20xx-xx-xx-raspbian-jessie-lite.img /mnt/image/boot
sudo mount -v -o offset=67108864 -t ext4 20xx-xx-xx-raspbian-jessie-lite.img /mnt/image/root
```

The output will hopefully show the following messages. If an error message appears, check the values you calculated.

```bash
mount: /dev/loop0 mounted on /mnt/image/boot.
mount: /dev/loop1 mounted on /mnt/image/root.
```

It is now time to make all the changes you require to the image. I’ll detail some of the changes I make below, mainly for my own documentation. When you are finished, the partitions will need unmounting. This can be achieved using the following commands. If you receive an error message, it could be that the folder is still being used by a program or the current folder in a terminal window.

```bash
sudo umount /mnt/image/boot
sudo umount /mnt/image/root
```

My Changes to Raspbian
These are the changes that I make to my Raspbian image. I have wireless adapters that need the power saving disabled to allow them to work properly. The folder paths are relative to /mnt/image/ on the Linux computer. The Linux computer has an SSH Public Key for passwordless SSH access, which was created using `ssh-keygen -t rsa -b 2048`.

In boot/config.txt, remove the hash to enable `dtparam=i2c_arm=on`.
In boot/config.txt, add the line `gpu_mem=16` as I only require 16MB of GPU memory.
In /etc/modules, add the line `i2c-dev` to automaticallty load the I2C driver.

Create the file root/etc/modprobe.d/8192cu.conf with contents of `options 8192cu rtw_power_mgnt=0 rtw_enusbss=0`.

In root/etc/network/interfaces, remove any eth0, wlan0 and wlan1 configuration. Add the following networking configuration, editing the following text to work in your environment. I change the default IP address manually every time on first boot. It’s nice to have a predictable first boot IP address, rather than using nmap to find it. In the future, I will be using mDNS and DHCP.

```
iface eth0 inet static
address <DefaultIPAddress1>
netmask <SubnetMask>
gateway <GatewayIPAddress>

auto wlan0
iface wlan0 inet static
wpa-ssid <SSID>
wpa-psk <WPA2Key>
address <DefaultIPAddress2>
netmask <SubnetMask>
gateway <GatewayIPAddress>
wireless-power off
```

Create an .ssh folder in root/home/pi/ using `sudo mkdir -p root/home/pi/.ssh`.

Add your SSH Public Key from /home/<username>/.ssh/authorized_keys using `sudo cat /home/<username>/.ssh/id_rsa.pub >> root/home/pi/.ssh/authorized_keys`. Enter your Linux computer username into the command first.

Check the user id of the files within root/home/pi/ using `ls -lan root/home/pi/` and set the owner and group to these same values using `sudo chown -R 1000:1000 root/home/pi/.ssh`. The user id numbers may be different.

Create a password for user pi using `mkpasswd -m sha-512 <newpassword> <salt>`. The salt value is a random string of your choice, anything will do.

Change the root/etc/shadow file with the new password above. Delete the string between the 1st and 2nd ‘:’ colons on the line starting with ‘pi’. Paste in the new value between these colons.

You should now have a configured Raspbian image for use on your Raspberry Pi computers.

This post was inspired by a [Stack Exchange forum post](http://raspberrypi.stackexchange.com/questions/13137/how-can-i-mount-a-raspberry-pi-linux-distro-image).
