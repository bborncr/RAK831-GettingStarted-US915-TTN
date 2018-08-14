# RAK831-GettingStarted-US915-TTN

This tutorial takes you through all the steps required to configure the **RAK831 US915 Kit 7** as a full 8 channel LoraWAN gateway connected to **The Things Network** on the US915 band. This tutorial assumes that you will be using **Ethernet using DHCP** to connect to the internet. There are many tutorials online that show how to switch over to WiFi or use static IPs if you wish to do so in the future.

Although this version of the kit comes with some software pre-installed and pre-configured, there are certain changes and additional setup required in order to complete the process.
This tutorial assumes that you have the **RAK831 US915 Kit 7**. This particular kit comes with the following:
* RAK831 radio module
* Converter board with GPS module
* GPS antenna
* 915MHz indoor antenna
* SD card pre-configured with the TTN gateway software pre-installed
* Raspberry PI 3

The kit requires a **2.5A 5V power supply**. Any less current and it is possible that the processor could throttle down and have problems.

## Steps required for configuration
1. Setup/install the kit.
1. Power up.
1. Find the IP address.
1. Login using SSH, remove the gateway-remote-config directory, create the Gateway ID.
1. Register a new gateway on The Things Network.
1. Replace the default `global_config.json` file with the US915 version.
1. Setup the `local_config.json` file with your specific gateway information.

### Setup/install the kit
* Plug in the antenna. Note that powering on the RAK831 gateway without the top antenna may damage the radios. **The GPS antenna is optional.**

![Antenna](https://github.com/bborncr/RAK831-GettingStarted-US915-TTN/blob/master/images/antenna.PNG)

### Power up
* Connect the Raspberry PI to your router via Ethernet and power up. Make sure that the power supply is 2.5A or more.
### Find the IP address
* After waiting 30 seconds, the network should have assigned an IP address. There are several methods to find the IP address without connecting a keyboard and monitor depending on your operating system. [Here is the link to the official Raspberry Pi documentation.](https://www.raspberrypi.org/documentation/remote-access/ip-address.md)
### Login using SSH, remove the gateway-remote-config directory
* SSH into the Raspberry Pi. The default user is `pi` and the default password is `11111111`. Change the password immediately to something secure. 
* Remove the `gateway-remote-config` directory. If we don't remove this directory then the start files will overwrite our local_config.json
```
cd /opt/ttn-gateway
sudo rm -rf gateway-remote-config
```
* Find your MAC address and create the Gateway ID
The Gateway ID can be any 8 byte hexadecimal that you like, however, an unofficial convention is to use the physical MAC address to generate the Gateway ID. The RAK831 comes pre-configured but we want to overwrite and make some changes. The convention that many are using to create the Gateway ID is to take the first three bytes of the hardware MAC address: for example `B827EB` add `FFFE` and then the remaining three bytes `AABB01`. So in this case the full Gateway ID would be `B827EBFFFEAABB01`. There should be 8 bytes for a total of 16 characters. Write this number down for later.
### Register a new gateway on The Things Network
* Log into your console on The Things Network.
* In `gateways` click on `register gateway`
* Select `I'm using the legacy packet forwarder`
* Copy your Gateway ID into the form
* Choose the US Frequency Plan and ttn-router-us-west
* Select the location of you gateway in the map
* Click on `Register Gateway`
### Replace the default `global_config.json` file with the US915 version
The default `global_config.json` file is for the European band. We need to replace this file with a US915 band version. Using the following three commands we move to the `/opt/ttn-gateway/bin` directory, then download the `US-global_conf.json` file and then finally replace the `global_config.json` file.
```
cd /opt/ttn-gateway/bin
sudo wget https://raw.githubusercontent.com/TheThingsNetwork/gateway-conf/master/US-global_conf.json
sudo cp US-global_conf.json global_config.json
```

### Setup the `local_config.json` file with your specific gateway information
The following commands changes the directory, downloads the template file and renames it as `local_conf.json` and then open into `vi` for editing. 
```
cd /opt/ttn-gateway/bin
sudo wget https://raw.githubusercontent.com/ttn-zh/gateway-remote-config/master/template.json
sudo cp template.json local_conf.json
sudo vi local_conf.json
```
Make the following modifications:
* add your Gateway ID
* change the `eu` to `us` in the `server_address`
* add the lat, long, altitud of the gateway
* add your contact email and description
* save and exit `vi`

Reboot the Raspberry PI using `sudo shutdown -r now`

In a few minutes the gateway should appear as **connected** in the TTN console.
The gateway logs can be seen using `sudo tail -f /var/log/syslog`

