# Headless Setup of ROC-RK3328-CC with Raspbian + Pihole

## References
- [Raspbian images](https://distro.libre.computer/ci/raspbian/11/)
- [Installing Raspbian on an SD Memory Card](https://kalitut.com/installing-raspbian-on-sd-card/)
- [Raspbian 11 Bullseye for Libre Computer Boards](https://hub.libre.computer/t/raspbian-11-bullseye-for-libre-computer-boards/82)
- [An update to Raspberry Pi OS bullseye](https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/)
  - **'pi' user has been removed!**
- [How can I mount a Raspberry Pi Linux distro image?](https://raspberrypi.stackexchange.com/questions/13137/how-can-i-mount-a-raspberry-pi-linux-distro-image)

## Pre-requisites
- xz compression utility
`apt install xz-utils`


## Manual Installation and Setup

**Instructions are to be performed on linux!**

1. Download image
	`wget https://distro.libre.computer/ci/raspbian/11/2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img.xz`
1. uncompress image
	`unxz -k 2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img.xz`
1. mount image
```
	losetup -P /dev/loop0 2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img
	mkdir /mnt/raspbian
	mount /dev/loop0p2 /mnt/raspbian
	mount /dev/loop0p1 /mnt/raspbian/boot
```
1. configure user
1. Locate sdcard device
   1. lsblk
1. using sdd/sdd1 as example
`xzcat 2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img.xz | dd status=progress bs=4M of=/dev/sdd1`

	root@raymond-main:/home/rtai/Projects/RaySirX/rk3328-pihole# xzcat 2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img.xz | dd status=progress bs=4M of=/dev/sdd1
	1526521856 bytes (1.5 GB, 1.4 GiB) copied, 55 s, 27.8 MB/sxzcat: 2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img.xz: Compressed data is corrupt

	0+184124 records in
	0+184124 records out
	1531807782 bytes (1.5 GB, 1.4 GiB) copied, 55.2918 s, 27.7 MB/s
	root@raymond-main:/home/rtai/Projects/RaySirX/rk3328-pihole# sync
	root@raymond-main:/home/rtai/Projects/RaySirX/rk3328-pihole#

## Using Bash scripts installIt.d/*

1. configure defaults
  - DEFAULT\_PI\_REPO - http endpoint and path to raspbian image
  - DEFAULT\_PI\_DISTRO - image filename
  - DEFAULT\_PI\_DISTRO\_LOCAL\_DIR - download destination directory of the image
  - DEFAULT\_PI\_DISTRO\_MOUNT\_DIR - mount point directory where image will be mounted
  - DEFAULT\_PI\_USER - initial user account to be created - see .secrets:DEFAULT\_PI\_USER\_PASS
  - DEFAULT\_PI\_SSID - wifi network name - see .secrets:DEFAULT\_PI\_SSID\_PASS

`source .defaults`

1. configure .secrets
  - DEFAULT\_PI\_SSID\_PASS - wifi password
  - DEFAULT\_PI\_USER\_PASS - initial user password

`source .secrets`

1. Download image
`./installIt.d/01-download-raspbian-image`

1. Mount image file
`./installIt.d/02-mount-raspbian-image ${DEFAULT\_PI\_DISTRO\_LOCAL\_DIR?}/${DEFAULT\_PI\_DISTRO?}`

1. Setup default user
`./installIt.d/10-raspbian-setup-default-user`

1. Setup wireless
`./installIt.d/15-setup-wireless`

1. Enable ssh
`./installIt.d/20-enable-ssh`

1. Set hostname
`./installIt.d/25-set-hostname`

1. Unmount image
`./installIt.d/99-unmount-raspbian-image`

# Copy image to SD card
`dd bs=4M if=/dev/loop0 of=/dev/sdd status=progress`
*DOUBLE DOUBLE CHECK the output device!*
