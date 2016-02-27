# RPi


Pi_Duo Project


1. Prerequisites

	Install Noobs 1.7.0 (Linux kernel version 4.1.17) first:

		https://www.raspberrypi.org/downloads/noobs/

	Note: this package is only for kernel 4.1.17
	
	
2. WiFi

	Edit config.txt lacated in the SD Card (e.g. with a SD card reader).

	Add the following line to the file:

    	dtoverlay=sdio,poll_once=on

	Edit cmdline.txt, remove
  
		console=ttyAMA0,115200

	Boot into the RPi (X-Window), you should be able to associate an WiFi AP.

	Note: if you do not have a PC or SD card reader, boot into the RPi and use command line to do that:

		$ sudo nano /boot/config.txt

	You can check with the following command to see its IP address.

		$ ifconfig


3. Bluetooth

	After step 1, you should be able to get access to the Internet.
  
	Download this repository, copy and replace the files inside the Pi_Duo folder to your RPi file system.

	Change mode to some files and update rc.d:
  
		$ sudo chmod +x /usr/bin/brcm

		$ sudo chmod +x /etc/init.d/brcmbt
  
		$ sudo update-rc.d -f brcmbt defaults

	Install BlueZ

		$ sudo apt-get install bluez

	Install Bluetooth Manager

		$ sudo apt-get install blueman

	Reboot the RPi and config your Bluetooth keyboard/mouse 


	Note: The blueman has problem to show the pairing key for keyboards to pair. Start a command line and use 'bluetoothctl', type the commands as shown: 

		$ sudo bluetoothctl
		[bluetooth]# agent KeyboardDisplay
		[bluetooth]# default-agent
	
	Then, use blueman to do the pairing but use the pairing key shown in bluetoothctl. See 'pair.jpg'.
	
	
	You should see hci0 interface is up with this command
  
		$ hciconfig


4. Testing

	3.1 To test iBeacon, see:
  
    	https://github.com/dburr/linux-ibeacon/
  
	3.2 To test WiFi speed:

		$ curl -O ftp.cuhk.edu.hk/pub/Linux/ubuntu-releases/15.10/ubuntu-15.10-desktop-amd64.iso

	Note: do the same command on OSX to compare.
    
	3.3 After WiFi setup, you can use mDNS to get the IP address of the RPi (e.g. no HDMI connection)
  
		$  dns-sd -B _workstation._tcp
    
	And then, you can use ssh to login to the RPi
    

5. Known Issues

	* The command 'reboot' will not work for the PiDuo, after reboot, the PiDuo will not work properly, e.g.
	
			$ sudo reboot  

	* Workaround: use the 'shutdown' command and remove the power from the RPi
	
			$ sudo shutdown now
			
	* This will be fixed in next board desgin by adding a GPIO to reset the PiDuo.
	
