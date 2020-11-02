# projectorRS232
On/Off control of UHD30 via RS232 Interface

The goal of this project was to emulate HDMI CEC functionality on the Optoma UHD30 projector. This projector does not support on/off commands from devices attached to the HDMI port. Ideally a single remote (AppleTV remote in my case) is all that is needed to turn everything on and off. I accomplished this by using a Raspberry Pi 3 connected to the projector via a USB to RS232 cable/adapter (https://www.amazon.com/gp/product/B07GNKMHFW/). The Raspberry Pi runs Node-RED which monitors the state of the AppleTV via a homebridge plugin. When the Apple TV is on it sends the On command to the projector and when the AppleTV is off it sends the off command. While this is a rather simple and straightforward automation, I found a lack of info online about various aspects of this project. Documenting my efforts here I hope will help others. 

## Setup
I started with a fresh install of Raspberry Pi OS installed on an SD card. I opted to use the minimal install, since I don't plan to run any desktop environment. After installing I enabled SSH, and installed Node-RED with the command:
```
$ apt-get install nodered
```
Auto Start Node-RED at boot:
```
$ sudo systemctl enable nodered.service
```
More info on Node-RED can be found here: https://nodered.org/docs/getting-started/raspberrypi
Once you have Node-Red running open a browser on your computer and go to http://IP.ADDRESS.RPI:1880 to get the web UI for designing a flow.

