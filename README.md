# SyntrekWifi
A Wifi interface to replace your Syntrek hand control. This can then be used by other applications to control your telescope.

## Introduction
This is supposed to be a recipe of how to replace the hand control of your Skywatcher dobsonian telescope with a Wifi module. This will allow you to control the telescope from your Android phone and have goto as well as tracking abilities.

Here is a picture of the original Syntrek hand control:
![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/Syntrek-HC.jpg "The old Syntrek hand control")

This modification will also work if you are using a Synscan hand control, as it will also replace the Synscan. If you want to still use the Synscan hand control, I do not have one, so I cannot say if it will work or not. Here is a picture of the Synscan AZ hand controller: 

![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/Synscan-HC.jpg "The old Synscan hand control which can also be replaced")

Here is a picture of the new SyntrekWifi before it was enclosed in a case:
![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/SyntrekWifi-front-no-case.jpg "Front of our new SyntrekWifi module")

## CAUTION
<b>Please note that I do not take responsibility for anything that happens. This is a document for my own use, to remind me of the steps I have taken to get my telescope working.</b>

## Background
<b>Note: You can skip this section if you want to dive in head first and just follow the steps!</B>

I searched a lot on the internet, and found many people who automated the Skywatcher equitorial mounts, and some who automated their dobsonian mounts via a synscan controller, but none who automated their dobsonian without a Synscan controller.

I did some reverse engineering by opening the controller on the dobsonian mount as well as the Syntrek hand controller.
The controller on the mount:
![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/mount-pcb-front.jpg "Front of mount PCB")
![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/mount-pcb-back.jpg "Back of mount PCB")

Here is a picture of the SynTrek hand control PCB:
![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/Syntrek-HC-back.jpg "Back of hand control PCB")

The thing that took me the most time to understand was that I did not realise that Synta are connecting the TX and RX lines together and then communicating in a simplex way instead of a normal duplex transmission. This can easily be seen on the picture of the back of the mount PCB, where you can see that pin 3 and 5 are connected together.

Luckily I found a web page of another hacker who reversed engineered the SupaTrak Mount, which is very similar although it uses older PIC microcontrollers: https://sites.google.com/site/rwgastro/other-stuff/supatrakmounthacking
He also reversed engineer the protocol. You can read more about it in: https://github.com/Orgjvr/SyntrekWifi/blob/master/resources/SupaTrakMountHacking.pdf
This really helped me with the last step I needed. It seems to be rwg if I read this post: https://stargazerslounge.com/topic/47354-goto-for-skywatcher-flextube-auto-dobs-is-a-reality/?do=findComment&comment=592621 Thanks rwg!

Thus the pin-out of the RJ12 socket on the mount is as follows (with pin 1 being closest to the RJ45 connector):
![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/rj12.png "RJ12 Pin numbers")
- Pin 1: Flow control line. We are not using it.
- Pin 2: GND - Ground
- Pin 3: Data - Both TX and RX connected.
- Pin 4: Vin - This is the power supplied to the mount. Thus if 12V battery is used, then this will be 12V.
- Pin 5: Data - Both TX and RX connected.
- Pin 6: NC - Not connected

Skywatcher has also open sourced their API: https://github.com/skywatcher-pacific/skywatcher_open
Thanks Skywatcher! We really appreciate it.

## What you need
- RJ12 connector
- A piece of 6 core wire (4 core can work in a pinch). 
  - I used a short 10cm piece: https://www.aliexpress.com/store/product/1meters-lot-6Pin-Grey-Flat-Electrical-Wire-Cable-6-cores-Telephone-Wires-With-6P-28AWG-Electrical/123320_32586504070.html
- DC-DC buck (step down) power supply
  - I used an DSN5000 XL4005 module https://www.aliexpress.com/item/1PCS-XL4005-DSN5000-Beyond-LM2596-DC-DC-adjustable-step-down-power-Supply-module-5A-High-current/1994561242.html
- ESP TTL-Wifi module
  - I bought a Geekcreit® DT-06 Wireless WiFi Serial Port module https://www.banggood.com/Geekcreit-DT-06-Wireless-WiFi-Serial-Port-Transparent-Transmission-Module-TTL-To-WiFi-p-1141047.html?cur_warehouse=CN
  - A wiki about this module: https://github.com/SmartArduino/SZDOITWiKi/wiki/Basic-Manual
- 4 Pin Dupont connector
  - I used a 4 pin cable which I cut in half: https://www.aliexpress.com/store/product/CHANGTA-5pcs-lot-70cm-4-Pin-Female-to-Female-Jumper-Wire-Dupont-Cable-70CM-4pin-Jumper/2345289_32845946980.html
  - You can also use 4 single pin connectors: https://www.aliexpress.com/store/product/40pcs-dupont-cable-jumper-wire-dupont-line-female-to-female-dupont-line-20cm-1P-40P-free/1252082_32848338475.html
  - Or even just solder the wires to the TTL-Wifi module.
- A small box to contain everything and keep it safe.

## Tools needed
  - Multimeter
  - Soldering iron with solder
  - Pliers to cut the wires
  - A knife to open the isolation on the wire
  - A crimping tool (This is optional)
    - You can always cut a piece from an old phone. Just make sure that it has at least 4 wires inside.
  
## Lets build it!
#### Step 1
Before we blow anything up, we need to set the output of the PowerSupply module to be as near as possible to 5V. This means anything between 4.8V and 5.2V. We can set the output by connecting the input side of module to the power supply or battery you are normally using with your telescope. Use the multimeter and measure the output side of the module and ensure it is at 5V. If it is not, then adjust it until it is 5V.

#### Step 2
Crimp the RJ12 connector to the wire. See the picture below to understand the pin numbering I am using further on in this instructions: 

![alt text](https://github.com/Orgjvr/SyntrekWifi/blob/master/pics/rj12.png "RJ12 Pin numbers")

#### Step 3
Solder the power from the mount to the input of the Power supply module. Pin 4 is the positive and pin 2 is the ground or negative. <b>IMPORTANT: Please ensure that you take the correct wires as this can cause damage if you use incorrect wires or short these 2 wires</b> You can test these wires to ensure you are using the correct wires, by measuring the voltage between these 2 wires, while connected to the base and the base must be switched on and powered. If you power the base by 12V, then you need to read 12V on these 2 pins.

#### Step 4
Solder the power from the TTL-Wifi module to the output of the Power supply module. The positive from the PSU goes to the VCC on the TTL-Wifi module and the ground on the PSU goes to the GND on the TTL-Wifi module.   

#### Step 5
Connects RXD and TXD on the TTL-Wifi module to the cable from the mount to pin 3 and 5. Any of the 2 pins can be connected to either the TXD or RXD, as they are connected together internally on the mount. For those who are unsure, connect pin 3 to TXD and then connect pin 5 to RXD.

#### Step 6
Put everything in a nice plastic container, and take precaution that nothing will create a short circuit.

#### Step 7
Congratulations! You are done with the hardware. Now you can remove the hand controller from the mount and replace it with the new Wifi module. Power the telescope. Now we need to do a few settings on the TTL-Wifi module.

## Lets try to use it!
### Setup TTL-Wifi module
Connect to the SyntrekWifi with a Wifi enabled computer, phone or tablet. Look for the SSID which is something like "Doit_WiFi_xxxxxx" with x being a unique identifier. In case your phone tells you that you do not have internet connection, just accept it and tell it to use the network as is. By default the IP address of the telescope will be 192.168.4.1. 
Open your browser and enter 192.168.1.4 in the address field. This will connect to a web page that runs on the TTL-WiFi module. Now we need to change some settings:
 Server type must be "UDP Server".
 UDP Server must be 11880
 Leave the rest as is.

You can change the Wifi SSID to SyntrekWifi or anything else if you want. 

<b>Ensure you reboot the module before trying to set up any further software.</b>

### Android
There is a few options on Android, but I only tried Synscan Pro and Skysafari 5 Plus.

#### Synscan Pro
Synscan Pro can be downloaded from the Google Play store: https://play.google.com/store/apps/details?id=com.skywatcher.synscanapppro

Synscan Pro will look for a UDP service running on port 11880. Thus is why we picked these settings above. Currently this cannot be changed in Synscan Pro version 1.9.0.

One thing that I picked up, is that if I manually move the telescope, then Syncan Pro will not show the new direction in which it is pointing, until you click on the refresh button. I think this is a limitation of the software on the mount PCB. I need to still hook it up to the oscilloscope to test this.

#### Skysafari 5
You need at least SkySafari 5 plus if you want to control the telescope: https://play.google.com/store/apps/details?id=com.simulationcurriculum.skysafari5plus
Skysafari 5 will not connect directly to SyntrekWifi, but it will connect via Synscan Pro. Thus we first needs to open Synscan Pro and let it connect to the telescope. Then we can connect to it via Skysafari 5.
The settings in Skysafari 5:
- Under Settings/Telescope/Setup:
  - Select "SkyWatcher SynScan" under Scope Type.
  - Select your mount type. Mine is: "Alt-Az. GoTo."
  - Select the "Connect via WiFi" option.
  - At the IP Address enter: 127.0.0.1
  - And the Port Number must be 11882.

Now SkySafari 5 will be able to connect to the telescope. 

### IOS
Until someone come around and create this section, you will be on your own. Sorry.

PS: Get a Android phone or table and dedicate it to the telescope :p

### Linux
I planned on controlling the telescope from my Linux laptop, but the Android control is working so nicely, that I am not sure whether I will investigate this anymore. Please let me know if you want to do a writeup for this.

### Windows
Until someone come around and create this section, you will be on your own. Sorry.

## To do
- Create a small case
- Take a picture of the completed project.
