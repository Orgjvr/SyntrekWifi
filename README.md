# SyntrekWifi
A Wifi interface to replace your Syntrek hand control. This can then be used by other applications to control your telescope.

## Introduction
This is supposed to be a recipy of how to replace the hand control of your Skywatcher dobsonian telescope with a Wifi module. This will allow you to control the telescope from your Android phone and have goto as well as tracking abilities.

## CAUTION
<b>Please note that I do not take responsibility for anything that happens. This is a document for my own use, to remind me of the steps I have taken to get my telescop working.</b>

## Background
<b>Note: You can skip this section if you want to dive in heaad first!</B>

I searched a lot on the internet, and found many people who automated the Skywatcher equitorial mounts, and some who automated their dobsonian mounts via a synscan controller, but none who automated their dobsonian without a Synscan controller.

I did some reverse engineering by opening the controller on the dobsonian mount as well as the Syntrek hand controller.
The controller on the mount:



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
### Step 1
Before we blow anything up, we need to set the output of the PowerSupply module to be as near as possible to 5V. This means anything between 4.8V and 5.2V. We can set the output by connecting the input side of module to the power supply or battery you are normally using with your telescope. Use the multimeter and measure the output side of the module and ensure it is at 5V. If it is not, then adjust it until it is 5V.

### Step 2
Crimp the RJ12 connector to the wire. 

### Step 3
Solder the power from the mount to the input of the Power supply module. Pin 4 is the positive and pin 2 is the ground or negative.

### Step 4
Solder the power from the TTL-Wifi module to the output of the Power supply module. The positive from the PSU goes to the VCC on the TTL-Wifi module and the ground on the PSU goes to the GND on the TTL-Wifi module.   

### Step 5
Connects RXD and TXD on the TTL-Wifi module to the cable from the mount to pin 3 and 5. Any of the 2 pins will work, as they are connected together internally on the mount.

### Step 6
Put everything in a nice plastic container, and take precaution that nothing will create a short circuit.

### Step 7
Congratulations! You are done with the hardware. Now you can remove the hand controller from the mount and replace it with the new Wifi module. Power the telescope. Now we need to do a few settings on the TTL-Wifi module.

### Step 8
<i>I am assuming you are using an Android phone. Otherwise you need to use common sense to do the same on another phone or computer</i>
Open your Wifi settings on your phone and searh for <ADD SSID HERE>. Connect to it. In case your phone tells you that you do not have internet connection, just accept it and tell it to use the network as is. By default the IP address of the telescope will be 192.168.4.1.
  
## First use


## Software setup

## To do
