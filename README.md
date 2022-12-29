# Headless Setup of ROC-RK3328-CC with Raspbian + Pihole

## References
[Raspbian images] https://distro.libre.computer/ci/raspbian/11/
[Installing Raspbian on an SD Memory Card] https://kalitut.com/installing-raspbian-on-sd-card/
[Raspbian 11 Bullseye for Libre Computer Boards] https://hub.libre.computer/t/raspbian-11-bullseye-for-libre-computer-boards/82
[An update to Raspberry Pi OS bullseye] https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/
- **'pi' user has been removed!**
[How can I mount a Raspberry Pi Linux distro image?] https://raspberrypi.stackexchange.com/questions/13137/how-can-i-mount-a-raspberry-pi-linux-distro-image

**Instructions are to be performed on linux!**

1. `wget https://distro.libre.computer/ci/raspbian/11/2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img.xz`
1. apt install xz-utils
1. unxz apt-get install xz-utils
1. uncompress image
`unxz -k 2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img.xz`
1. mount image
	losetup -P /dev/loop0 2022-09-22-raspbian-bullseye-arm64+roc-rk3328-cc.img
	mkdir /mnt/raspbian
	mount /dev/loop0p2 /mnt/raspbian
	mount /dev/loop0p1 /mnt/raspbian/boot
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



