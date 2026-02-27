# VirtualLoop User Guide

## Table of Contents
1. [Introduction](#Introduction)
1. [Models](#Models)
1. [Interfaces](#Interfaces)
1. [Factory reset](#FactoryReset)
1. [Web Server](#WebServer)
1. [REST API](#RESTAPI)
1. [Script Generator for Bosch Cameras](#Scripts)
1. [Getting started checklist](#GettingStarted)
1. [Firmware upgrades](#Firmware)
1. [Contact](#Contact)

## Introduction<a name="Introduction"></a>

The VirtualLoop is a SDLC-to-Ethernet Bus Interface Unit device.  Tested with a variety of brands of 2070 and ATC controllers, the VirtualLoop is flexible to work in many cabinet environments.
	
### Intended use
	
The VirtualLoop serves as a translator for various detection technologies to be able to represent themselves to the traffic controller in a manner similar to inductive loops.  The sensor (camera, lidar, etc) places a call to the VirtualLoop REST API which is translated into calls through TS2 SDLC.
	
### Physical and Environmental Specs

* Temperature: -34C to +74C
* Humidity:	95% non-condensing
* Dimensions: 55 x 153 x 106mm
* Weight: 18oz
* Low power consumption: 1 Watt
		
## Models<a name="Models"></a>

### AT-STE-02-RACK: BIU Rack card mounted

**Power:** supplied by the BIU rack backplane at 12 VDC

**Mounting:**  BIU rack card

**Relay Control:**  None

<img height="300" src="https://github.com/user-attachments/assets/300a3dfd-6d98-4a05-8ba9-f6fcd87354bc" />

### AT-STE-02-DIN: DIN rail mounted

**Power:** supplied either by nominal 12-24VDC on the front (2 pos green connector), or Ethernet (RJ45) PoE 802.3af (consumes only ~1w)

**Mounting:** Din or shelf 2.1" wide x 6" deep x 4.1" high

**Relay Control:** 8 ea of SPDT relays (1 Form C)

<img height="300" src="https://github.com/user-attachments/assets/e5f4a593-da96-4832-865a-9d17f8a7e4b0" />

## Interfaces<a name="Interfaces"></a>

### Ethernet

- 100Mb Ethernet with PoE 802.3af
- IP addressing through DHCP and Static

### LEDs

* **(Green) Power:** Solid on when the unit is powered

* **(Orange) Heartbeat:** Blinks at 1Hz when the system is operating normally.  Fast blinking indicates an error

* **(Red) Network:** Blinks with each REST API call

* **(Yellow) SDLC:** Blinks during SDLC bus communication

### OLED Screen

<img width="273" height="157" alt="OLED" src="https://github.com/user-attachments/assets/26a85162-b11d-4007-9740-de64b0d2e4f5" />

Tapping the button rotates the screen.  See [Factory reset](#FactoryReset) for details about holding the button.

1. Network addressing (Static or DHCP)
1. About versions
1. BIU #1 and Detectors 1-16
1. BIU #2 and Detectors 17-32
1. BIU #3 and Detectors 33-48
1. BIU #4 and Detectors 49-64
1. Device status
1. Load switches
1. SD card status
1. Volts
1. Option switches
1. Failsafe monitor
1. Relays

### SDLC

TS2 Port 1 Serves as BIU 1-4, each with 16 detectors.  The BIU assignments and Detector calls can be operated through the Web Server "Detector" page and REST API.

### Relays (DIN Rail Model)

8 relays (C/NO/NC contacts) each can switch a max of 1A @ 125VAC or 60VDC

Relays are numbered 1 through 8 with 1 closest to the front of the unit.  Each relay includes Common, Normally Open, and Normally Closed contacts.

<img width="800" alt="Relay contacts" src="https://github.com/user-attachments/assets/189c1398-b71e-48e6-81df-30fbbb72ee31" />
	
## Factory reset<a name="FactoryReset"></a>

Holding the front button for 5 seconds (or longer) and releasing will factory reset the device.
	
## Web Server<a name="WebServer"></a>

**About page** - Reports version, internal voltage, device name, etc.

<kbd><img width="396" alt="2025-01-23 12_49_12-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/141838cd-6e46-40e5-a299-f4d499a906c1" /></kbd>

**Network configuration page** - Used to set the device IP through either DHCP or Static addressing

<kbd><img width="396" alt="2025-01-23 12_49_35-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/7da69c1c-1a48-4f9f-b282-35be76ae86cd" /></kbd>

**Detector selection page** - Allows the user to force a detector on or off through the UI.  Also reports the number of detector activation counts and provides the user a droplist of the load switch numbers for mapping in ATSPM data

<kbd><img width="396" alt="2025-01-23 12_49_56-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/ae04f788-682d-466b-9021-22971ee01b02" /></kbd>

**Failsafe monitoring page** - The device can monitor upto 4 IP address and ports.  Typically these would be the camera detection devices.  If the connection is not accepted within the specified number of seconds and failure counts, the VirtualLoop will go into FAILSAFE mode and automatically hold calls on all detectors.

<kbd><img width="396" alt="2025-01-23 12_50_30-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/163a9e9e-c5fa-4300-9b34-c5ed7b8b7cfa" /></kbd>

**System page** - System functions like name, reboot, and factory reset.

<kbd><img width="396" alt="2025-01-23 12_50_55-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/ae4f439e-28da-4d14-aabb-83732f7ddb76" /></kbd>

## REST API<a name="RESTAPI"></a>

The REST API supports integration with sensor systems through simple HTTP requests.  See this [[PostMan API](https://documenter.getpostman.com/view/2915446/2sAXqp8PEb)] documentataion for details.

## Script Generator for Bosch Cameras<a name="Scripts"></a>

Generates the Bosch camera alarm task scripts for ease of use and to reduce errors.

[[VirtualLoop Camera Script Generator 1.6 Portable Download](https://www.dropbox.com/scl/fi/ipz3sgbw7ufoh4nxb6kw0/STECameraScript_1.6.zip?rlkey=jdybqoive1tco27h8gxwwftua&dl=0)]

1. Enter the IP address of the VirtualLoop device
1. Fill out the table with the mappings of cameras and rules to BIUs and detectors
1. Hit the "Generate" button and now you have each of the script blocks
1. Paste accordingly into your camera's Alarm Task Editors

<img width="1235" alt="mappings" src="https://github.com/user-attachments/assets/dc0dad7b-f46c-40ca-8c7d-655c6e1b3928" />

## Getting started checklist<a name="GettingStarted"></a>

- [ ] Installation
	- [ ] Physical mounting on DIN, Rack, or Shelf
    - [ ] Power the device either through PoE, 12/24VDC plug, or BIU rack insertion
    - [ ] Verify the unit turns on and the heartbeat light blinks 1 per second
- [ ] Networking
    - [ ] Setup a laptop to communicate on the subnet 192.168.1.XXX and plugin to the front Ethernet port
    - [ ] The device initial IP address is indicated by the OLED screen
    - [ ] Login to the unit's web interface using a browser
    - [ ] Set the proper address, subnet, and gateway IP for the network which allows communication to the sensors
- [ ] Detector integration
    - [ ] Plugin the SDLC interface cable
    - [ ] Use the VirtualLoop Web interface to enable the proper BIU #'s
    - [ ] Verify communication with the Intersection Control Unit through LEDs and exiting failsafe mode
    - [ ] Use the [Script Generator for Bosch Cameras](#Scripts) to generate VCA tasks based on the rule and detector mappings
    - [ ] Install scripts onto the cameras as directed by the tool
- [ ] Additional Recommended Items
    - [ ] On the System tab, set a unique intersection name or number
    - [ ] Use the [Firmware upgrades](#Firmware) tool to install the latest firmware onto the device
    - [ ] Add failsafe monitoring configs
    - [ ] Add time synchronization configs
    - [ ] Rotate through the device screens checking for any errors

## Firmware upgrades<a name="Firmware"></a>

[[VirtualLoop Firmware Upgrade Utility 1.2 Portable](https://www.dropbox.com/scl/fi/rtb79ys46dydydu4ma76h/STEFirmwareUpgrader_1.2.zip?rlkey=xkjt9v6gsgjmpfusd72j7st51&dl=0)]

[[VirtualLoop Firmware 4.0](https://www.dropbox.com/scl/fi/6hm1cn5fp46efbhr9ixib/SAMD51_4.0.bin?rlkey=5hlgwmp0y6pt8ji4mz32iprl9&dl=0)]

Download the firmware upgrade utility below and follow the instructions on the screen.

<img width="484" height="476" alt="Firmware Upgrader 1 2 0 0" src="https://github.com/user-attachments/assets/a10b66ee-5bff-41fc-8377-57aa316f4482" />

You'll need a USB micro cable like this:

<img width="200" alt="USB micro" src="https://github.com/user-attachments/assets/48831168-136d-4534-9b12-10fafdf12587"/>

Double-click the reset button:

<img width="300" alt="FW reset button" src="https://github.com/user-attachments/assets/8fbc860e-803a-4031-bbb5-e74d15a9ecbe"/>

## Contact<a name="Contact"></a>

For assistance, please contact your distributor (preferred) or <brandon@apextalos.tech>