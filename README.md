You Are Here
============

You Are Here web server and repository for spoken stories [This description is FPO].

Support for You Are Here is provided by the Tow Center for Digital Journalism at Columbia University and the Knight Foundation. This code is published under the [AGPLv3](http://www.gnu.org/licenses/agpl-3.0.html).

Recommended Hardware
--------------------
[Raspberry Pi 3](https://www.adafruit.com/products/3055)  
[8GB MicroSD Card Class 10](https://www.amazon.com/gp/product/B00M55C0VU)  
[2.4A 5V Power Supply](https://www.adafruit.com/products/1995)  
T-Mobile network SIM card  
[Huawei E303 HSPA+ 3G GSM USB Wireless Mobile Broadband Modem](https://www.amazon.com/gp/product/B00C0E946U)    
[9db Antenna](https://www.amazon.com/gp/product/B00DMJI9TA)  
[WiFi Amplifier](https://www.amazon.com/gp/product/B00S4RIKK8)  
[TP-Link Wireless N150 Access Point, 2.4Ghz 150Mbps](https://www.amazon.com/gp/product/B002YETVVE/)  
[Pigtail Cables](https://www.amazon.com/gp/product/B00C5WZ1VE/)  

How to install  
--------------
First, be sure to download the latest Jessie or Jessie Lite image from the Raspberry Pi [downloads](https://www.raspberrypi.org/downloads/raspbian/) page to install on your SD card. If you download Jessie Lite, you will need to install git before proceeding:  

        sudo apt-get install git  
        
Assuming you are starting with a fresh [Raspbian](http://www.raspberrypi.org/downloads/) install, these are the steps for installing You Are Here on your Raspberry Pi. It is also assumed that you have one wireless USB adapter and one Huawei E303 3G Modem attached to your RPi's onboard USB ports. The wireless radio must work with the nl80211 driver.

* Set up your Raspberry Pi with a basic configuration

        sudo raspi-config

* Clone the repository into your home folder (assuming /home/pi)

        git clone https://github.com/TowCenter/YouAreHere.git && cd YouAreHere

* Run the installation script

        sudo ./install.sh

The installation process takes about 15 minutes. During installation, you will be prompted to name your wireless access point (default is YouAreHere). After install has completed, you will have a running lighttpd web server, a connection to a 3G data network and will be broadcasting a wireless access point. Connecting to the network and navigating to http://192.168.100.1 in a browser window will take you to the root of the web server, currently served from /var/www/.

After the installation script completes, you must SSH into the RPi and do two things: 1/ stop dnsmasq and 2/ manually set the routing gateway to that of the 3G modem (this will be fixed eventually):

        sudo /etc/init.d/dnsmasq stop
        sudo route del default
        sudo route add default gw 10.64.64.64

Notes
-----
* Recommended to *not* use a Broadcom WiFi radio, as this is the default radio on the Raspberry Pi 3 and all Broadcom radios are blacklisted by our script blacklist-bc.conf in order to let the external TP Link radio be wlan0.

* If you update the web site build files, rsync it to the document root in /var/www/ :  

        sudo rsync -r www/build/* /var/www/html/
        
* Recommended to reboot the device every 24hrs via a cron job if left unattended for long periods of time (to deal with system crashes). To set a cron job rebooting the device once a day, run the following command:

        sudo crontab -e

* Add to the crontab:

        reboot pi every 24hrs at 5a  
        0 5 * * * reboot -f
	
References
----------
* [You Are Here website](http://youarehere.network/)
* [Tow Center](http://towcenter.org/)
* [lighttpd](http://www.lighttpd.net/)
* [Using a Raspberry Pi with a 3G USB Stick](http://copyndone.com/2015/06/27/guide-how-to-use-raspberry-pi-with-3g-usb-stick/)
* [Raspberry Pi](http://www.raspberrypi.org/)
