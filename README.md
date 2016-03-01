# RPi


Pi_Duo Project


1. Prerequisites

	Install NOOBS 1.8.0 (Linux kernel version 4.1.18) first:

		https://www.raspberrypi.org/downloads/noobs/

	Note: this package is only for kernel NOOBS 1.8.0 or above
	
	
2. WiFi

	Boot into the RPi and use command line to edit config.txt:

		$ sudo nano /boot/config.txt

	Add the following line to the file:

		dtoverlay=sdio,poll_once=on

	Edit cmdline.txt,
	
		$ sudo nano /boot/cmdline.txt

	Remove
  
		console=ttyAMA0,115200

	Reboot (remove power) into the RPi (X-Window), you should be able to associate an WiFi AP.

	You can check with the following command to see its IP address.

		$ ifconfig


3. Bluetooth

	After step 2, you should be able to get access to the Internet.
  
	Download this repository, copy two files (brcm and brcmbt) to your Pi.

	Put `brcm` to folder /usr/bin
	
		$ sudo mv brcm /usr/bin
	
	Put `brcmbt` to folder /etc/init.d
	 
		$ sudo mv brcmbt /etc/init.d
		
	Change mode to these two files and update rc.d:
  
		$ sudo chmod +x /usr/bin/brcm

		$ sudo chmod +x /etc/init.d/brcmbt
  
		$ sudo update-rc.d -f brcmbt defaults

	Install Bluetooth Manager (if you need to use GUI for Bluetooth)

		$ sudo apt-get install blueman

	Reboot the RPi, you should see hci0 interface is up with this command
  
		$ hciconfig
		
	Config your Bluetooth keyboard/mouse using the Bluetooth icon near the clock (upper-right corner).

	Note: The blueman has problem to show the pairing key for keyboards to pair. Start a command line and use 'bluetoothctl', type the commands as shown: 

		$ sudo bluetoothctl
		[bluetooth]# agent KeyboardDisplay
		[bluetooth]# default-agent
	
	Then, use blueman to do the pairing but use the pairing key shown in bluetoothctl. See 'pair.jpg'.


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
	
