# RPi


Pi_Duo Project


1. WiFi

Edit config.txt lacated in the SD Card (e.g. with a SD card reader).

Add the following line to the end:

dtoverlay=sdio,poll_once=on

Edit cmdline.txt, remove console=/dev/tty.AMA0

Boot into the RPi, you should be able to associate an WiFi AP.

Note: if you do not have a PC or SD card reader, boot into the RPi and use command line to do that:

$ sudo nano /boot/config.txt

You can check with the following command to see its IP address.

$ ifconfig


2. Bluetooth

After step 1, you should be able to get access to the Internet.

Use rpi-update to update your kernel to the latest version (e.g. 4.1.15)

$ sudo rpi-update

Download this repository, copy and replace the files inside the Pi_Duo folder to your RPi file system.

Inside /etc/init.d

$ sudo update-rc.d -f brcm43438 defaults

Install bluez

$ sudo apt-get install bluez

Install Bluetooth Manager

$ sudo apt-get install blueman

Reboot the RPi and config your Bluetooth keyboard/mouse 

