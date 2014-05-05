# rpi-image-creator

See [Schlomos Blog](http://blog.schlomo.schapiro.org/2013/12/automated-raspbian-setup-for-raspberry.html) for more details about this project.

If you have a lot of devices and need to provision the SD cards for them then you don't want any manual configuration, you want a fully automated solution. We use DEB packages to customize our systems. PCs and Laptops get installed with the help of a [preseed](https://wiki.debian.org/DebianInstaller/Preseed) file. Since Raspberry Pis don't do PXE boot this project is our replacement of preseed for Raspian.

Automatically download Raspbian and create a customized image:

* Copies your SSH keys into root account and disables SSH password-based logins
* Removes Raspian first-run stuff so that the first boot of the device will be production ready
* Adds additional DEB repos and GPG keys
* Installs additional DEB packages
* Installs auto-upgrade cron job

This script does not simply copy a binary image file onto the SD card. Instead it modifies the Raspbian image in a chroot environment, partitions and formats the SD card with the required file systems and then copies the *files* to the SD card. As a result the file systems on the SD card always fill it up completely, without resizing.

## Installation

1. Clone/Checkout this project
1. Install required dependencies:
   * **Ubuntu**: `sudo apt-get install kpartx qemu-user-static`


## Usage

* Insert an SD card
* Find out the block device of the SD card in `/proc/partitions`, e.g. `/dev/mmcblk0`
* Run script: `rpi-image-creator /dev/mmcblk0`.

## Advanced Usage

* Dry-run script with `rpi-image-create --chroot`. This does everything except writing to an SD card. Instead it will put you into a chroot in the created image. Note: When you exit the chroot shell the chroot area is removed.
