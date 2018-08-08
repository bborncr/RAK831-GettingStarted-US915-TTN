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
1. Find the IP address.
1. Power up and login using SSH.
1. Register a new gateway on The Things Network.
1. Replace the default `global_config.json` file with the US915 version.
1. Setup the `local_config.json` file with your specific gateway information.

### Setup/install the kit
#### Plug in the antenna
Please note that powering on the RAK831 gateway without an antenna may damage the radios. **The GPS antenna is optional.**

![Antenna](https://github.com/bborncr/RAK831-GettingStarted-US915-TTN/blob/master/images/antenna.PNG)

### Register a new gateway on The Things Network
### Replace the default `global_config.json` file with the US915 version
The default `global_config.json` file is for the European band. We need to replace this file with a US915 band version. Using the following three commands we move to the `/opt/ttn-gateway/bin` directory, then download the `US-global_conf.json` file and then finally replace the `global_config.json` file.
```
cd /opt/ttn-gateway/bin
sudo wget https://raw.githubusercontent.com/TheThingsNetwork/gateway-conf/master/US-global_conf.json
sudo cp US-global_conf.json global_config.json
```

### Setup the `local_config.json` file with your specific gateway information
The following commands changes the directory, copies the template file and renames it as `local_conf.json` and then open into `vi` for editing. 
```
cd /opt/ttn-gateway/bin
sudo cp /opt/ttn-gateway/gateway-remote-config/template.json .
sudo cp template.json local_conf.json
sudo vi local_conf.json
```
