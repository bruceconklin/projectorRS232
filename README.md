# projectorRS232
On/Off control of UHD30 via RS232 Interface

The goal of this project was to emulate HDMI CEC functionality on the Optoma UHD30 projector. This projector does not support on/off commands from devices attached to the HDMI port. Ideally a single remote (AppleTV remote in my case) is all that is needed to turn everything on and off. I accomplished this by using a Raspberry Pi 3 connected to the projector via a USB to RS232 cable/adapter (https://www.amazon.com/gp/product/B07GNKMHFW/). The Raspberry Pi runs Node-RED which monitors the state of the AppleTV via a homebridge plugin. When the Apple TV is on it sends the On command to the projector and when the AppleTV is off it sends the off command. While this is a rather simple and straightforward automation, I found a lack of info online about various aspects of this project. Documenting my efforts here I hope will help others. 

## Setup
Begin by connecting the RS232 cable to the projector, in my case the projector had male pins, so I picked an adapter that had a female connector. If your adapter has male pins then you'll need a cable with female ends on both sides to connect everything. Then plug the USB end of the adapter into any of the available USB ports on the Raspberry Pi. From this point you're done connecting things and we just have to setup a few things in software. 

I started with a fresh install of Raspberry Pi OS installed on an SD card. I opted to use the minimal install, since I don't plan to run any desktop environment. After installing I connected the Pi to my local Wifi Network, enabled SSH, and installed Node-RED with the command:
```
$ apt-get install nodered
```
Auto Start Node-RED at boot:
```
$ sudo systemctl enable nodered.service
```
More info on Node-RED can be found here: https://nodered.org/docs/getting-started/raspberrypi
Once you have Node-Red running open a browser on your computer and go to http://IP.ADDRESS.RPI:1880 to get the web UI for designing a flow.

## Raspberry Pi and USB Serial Devices
The USB-RS232 adapter should show up under /dev/ttyUSB0 on the Raspberry Pi. This is the address where we will issue the on/off commands. Set the Node-RED serial out node to this address. Set Baud rate to 9600, Data Bits to 8, Parity Bit to None, and Stop Bits to 1, leave everything else set to 'auto'. Under output, Add character to output messages shoudl be set to '\r'.

Also set a Node-Red serial in node to this address to recieve the response back from the projector. Dont skip this step, I had issues sending commands when I didn't have a serial in node set up. Connect the serial in node to a debug node and output the msg.payload to the console to aid in debugging. 

At this point you can test the setup using an inject node. Connect an inject node to the serial out node. The serial out node will send whatever is in the msg.payload to the projector. (A note about Projector ID: The Serial commands require you to specify the projector ID in each command. This is done because in theory you could connect multiple projectors to the same control device over a single RS232 cable. I set my projector ID to 01. I recommend you do the same or modify the serial commands to match whaever your projector ID is set to.)  For an Optoma UHD30 projector with ID set to 01 I use  the following commands:
```
Projector on:
~0100 1
Projector off:
~0100 2
```


