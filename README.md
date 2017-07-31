# RPi


IoT HAT Project


1. Prerequisites

	Install NOOBS:

		https://www.raspberrypi.org/downloads/noobs/

        There are 3 files to be changed:
	
		* /boot/config.txt
		* /boot/cmdline.txt
		* /usr/bin/btuart
		
2. WiFi

	Boot into the RPi and use command line to edit the config.txt:

		$ sudo nano /boot/config.txt

	Add the following lines to the file:

		dtoverlay=sdio,poll_once=on,sdio_overclock=20
		dtoverlay=gpio-poweroff,gpiopin=6,active_low=true

	Edit cmdline.txt,
	
		$ sudo nano /boot/cmdline.txt

	Remove
  
		console=serial0,115200

	Reboot (remove power)
	
	> You should be able to associate an WiFi AP using the GUI (X-Window) tool

	> You can also use this link for command line:
	
		https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md
	
	You can check with the following command to see its IP address.

		$ ifconfig


3. Bluetooth

	Run the following commands
	
		$ gpio -g mode 5 out
		$ gpio -g write 5 1
		
		$ /usr/bin/hciattach /dev/serial0 bcm43xx 921600 noflow -		

	Then, you should see hci0 interface is up with this command
  
		$ hciconfig
		
	Config your Bluetooth keyboard/mouse using the Bluetooth icon near the clock (upper-right corner).
	
	Modify the script to run the command at boot time
	
		$ sudo nano /usr/bin/btuart
		
	Change the last line with the following lines and the reboot it, e.g.
	
		/usr/bin/gpio -g mode 5 out
		/usr/bin/gpio -g write 5 1
		/usr/bin/hciattach /dev/serial0 bcm43xx 921600 noflow -

4. Testing

	4.1 To test iBeacon, see:
  
    	https://github.com/dburr/linux-ibeacon/
  
	4.2 To test WiFi speed:

		$ curl -O ftp.cuhk.edu.hk/pub/Linux/ubuntu-releases/15.10/ubuntu-15.10-desktop-amd64.iso

	Note: do the same command on OSX to compare.
    
	4.3 After WiFi setup, you can use mDNS to get the IP address of the RPi (e.g. no HDMI connection)
  
		$  dns-sd -B _workstation._tcp
    
	And then, you can use ssh to login to the RPi (enable ssh first using raspi-config)
    
		$ ssh pi@raspberrypi.local
		
5. Known Issues

	* The command 'reboot' will not work for the IoT HAT; after reboot, the IoT HAT will not work properly, e.g.
	
			$ sudo reboot  

	* Workaround: use the 'shutdown' command and remove the power from the RPi
	
			$ sudo shutdown now
			
	* This will be fixed in next board desgin by adding a GPIO to reset the IoT HAT.
	
