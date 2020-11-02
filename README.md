# projectorRS232
On/Off control of UHD30 via RS232 Interface

The goal of this project was to emulate HDMI CEC functionality on the Optoma UHD30 projector. This projector does not support on/off commands from devices attached to the HDMI port. Ideally a single remote (AppleTV remote in my case) is all that is needed to turn everything on and off. I accomplished this by using a Raspberry Pi 3 connected to the projector via a USB to RS232 cable/adapter (https://www.amazon.com/gp/product/B07GNKMHFW/). The Raspberry Pi runs Node-RED which monitors the state of the AppleTV via a homebridge plugin. When the Apple TV is on it sends the On command to the projector and when the AppleTV is off it sends the off command. While this is a rather simple and straightforward automation, I found a lack of info online about various aspects of this project. Documenting my efforts here I hope will help others. 

**Setup**
