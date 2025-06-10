# VirtualLoop

## Table of Contents
1. [Models](#models)
2. [Product information and datasheets](#datasheet)
3. [Getting Started Checklist](#started)
4. [Factory Reset](#reset)
2. [REST API](#restapi)
3. [Web Interface](#webui)
4. [Bosch Camera Script Generator Tool](#camerascript)
5. [Firmware Upgrades](#firmware)
6. [Contact](#contact)

## Models<a name="models"></a>

### BIU Rack card mounted
**Power:** supplied by the cabinet backplane

**Mounting:**  BIU rack plugin

**Relay Control:**  None

<img height="250" alt="Rack_iso" src="https://github.com/user-attachments/assets/616049a2-af66-47e1-9e1f-77e0dc958a87" />
<img height="250" alt="Rack_side" src="https://github.com/user-attachments/assets/996f6862-f143-4e1a-8806-8dd1fbb3bdfd" />
<img height="250" alt="Rack_front" src="https://github.com/user-attachments/assets/eba9d5de-2109-4dc7-9f3c-2a36058491d5" />

### DIN rail mounted
**Power:** supplied either by 12/24VDC on the front, or PoE 802.3af (consumes only ~1w)

**Mounting:** Din or shelf 2.1" wide x 6" deep x 4.1" high

<img height="250" alt="Din_iso" src="https://github.com/user-attachments/assets/e606902a-25fe-4a86-87e3-f913a458dfb3" />
<img height="250" alt="Din_rear" src="https://github.com/user-attachments/assets/dd634314-a87f-4709-be97-53b4329bdefe" />
<img height="250" alt="Din_side" src="https://github.com/user-attachments/assets/fb541cc3-cbdc-4cb6-96dd-edceb44bea2e" />
<img height="250" alt="Din_front" src="https://github.com/user-attachments/assets/d0578019-3367-4cdd-824d-ca40280487d6" />

**Relay Control:**  8 relays (C/NO/NC contacts) each can switch a max of 1A @ 125VAC or 60VDC
Relays are numbered 1 through 8 with 1 closest to the front of the unit.  Each relay includes Common, Normally Open, and Normally Closed contacts.

<img width="800" alt="Relay contacts" src="https://github.com/user-attachments/assets/189c1398-b71e-48e6-81df-30fbbb72ee31" />


## Product information and datasheets<a name="datasheet"></a>
<img width="723" alt="overview" src="https://github.com/user-attachments/assets/fdd604a2-de2c-4213-968b-2eb035a5c74a" />

Presentation on Intersection Detection and ATSPM Data Collection: [[Intersection Video Detection.pptx](https://github.com/user-attachments/files/19509660/Intersection.Video.Detection.pptx)]

VirtualLoop AT-STE-02 Datasheet: [[VirtualLoop Datasheet.docx](https://github.com/user-attachments/files/19509669/VirtualLoop.Datasheet.docx)]

SDLC protocol reference: [[NEMA TS2-2003.pdf](https://www.dropbox.com/scl/fi/l3es29g8ugdyiyf4ybhre/NEMA-TS2-2003.pdf?rlkey=pro3as7zwp3ee7hn7jcgo2vp0&dl=0)]

## Getting Started Checklist<a name="started"></a>

- [ ] Installation
    - [ ] Power the device either through PoE, 12/24VDC plug, or BIU rack insertion
    - [ ] Verify the heartbeat light blinks 1 per second
- [ ]. Networking
    - [ ] Setup a laptop to communicate on the subnet 192.168.1.XXX and plugin to the front Ethernet port
    - [ ] The device initial IP address is indicated by the OLED screen
    - [ ] Login to the unit's web interface using a browser
    - [ ] Set the proper address, subnet, and gateway IP for the network which allows communication to the cameras
- [ ] Detector integration
    - [ ] Plugin the SDLC interface cable
    - [ ] Use the VirtualLoop Web interface to enable the proper BIU #'s
    - [ ] Verify communication with the Intersection Control Unit through LEDs and exiting failsafe mode
    - [ ] Use the [Bosch Camera Script Generator Tool](#camerascript) to generate VCA tasks based on the rule and detector mappings
    - [ ] Install scripts onto the cameras as directed by the tool
- [ ] Additional Recommended Items
    - [ ] On the System tab, set a unique intersection name or number
    - [ ] Use the [Firmware upgrades](#firmware) tool to install the latest firmware onto the device
    - [ ] Add failsafe monitoring configs
    - [ ] Add time synchronization configs
    - [ ] Rotate through the device screens checking for any errors

## Factory Reset<a name="reset"></a>
Holding the front button for 5 seconds or longer will factory reset the device.

## REST API<a name="restapi"></a>
Supports placing calls on specific detector #, as well as other features

PostMan API tools: [[PostMan](https://documenter.getpostman.com/view/2915446/2sAXqp8PEb)]

## Web Interface<a name="webui"></a>
Status page - Reports version, internal voltage, device name, etc.

<kbd><img width="396" alt="2025-01-23 12_49_12-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/141838cd-6e46-40e5-a299-f4d499a906c1" /></kbd>

Network configuration page - Used to set the device IP through either DHCP or Static addressing

<kbd><img width="396" alt="2025-01-23 12_49_35-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/7da69c1c-1a48-4f9f-b282-35be76ae86cd" /></kbd>

Detector selection page - Allows the user to force a detector on or off through the UI.  Also reports the number of detector activation counts and provides the user a droplist of the load switch numbers for mapping in ATSPM data

<kbd><img width="396" alt="2025-01-23 12_49_56-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/ae04f788-682d-466b-9021-22971ee01b02" /></kbd>

Failsafe monitoring page - The device can monitor upto 4 IP address and ports.  Typically these would be the camera detection devices.  If the connection is not accepted within the specified number of seconds and failure counts, the VirtualLoop will go into FAILSAFE mode and automatically hold calls on all detectors.

<kbd><img width="396" alt="2025-01-23 12_50_30-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/163a9e9e-c5fa-4300-9b34-c5ed7b8b7cfa" /></kbd>

System page - System functions like name, reboot, and factory reset.

<kbd><img width="396" alt="2025-01-23 12_50_55-Apex Talos VirtualLoop" src="https://github.com/user-attachments/assets/ae4f439e-28da-4d14-aabb-83732f7ddb76" /></kbd>

## VirtualLoop Script Generator for Bosch Cameras<a name="camerascript"></a>

[[VirtualLoop Camera Script Generator 1.5 Portable](https://www.dropbox.com/scl/fi/5tcepr2p8h2vbh11wa8fn/STECameraScript_1.5.zip?rlkey=fw5b4kg6uk1qw574nfql9krn1&dl=0)]

Generates the Bosch camera alarm task scripts for ease of use and to reduce errors.

1. Enter the IP address of the VirtualLoop device
2. Fill out the table with the mappings of cameras and rules to BIUs and detectors
3. Hit the "Generate" button and now you have each of the script blocks
4. Paste accordingly into your camera's Alarm Task Editors

<img width="1235" alt="mappings" src="https://github.com/user-attachments/assets/dc0dad7b-f46c-40ca-8c7d-655c6e1b3928" />

## Firmware Upgrades<a name="firmware"></a>

[[VirtualLoop Firmware Upgrade Utility 1.1 Portable](https://www.dropbox.com/scl/fi/y765anx270u9r2flyi9zm/STEFirmwareUpgrader_1.1.zip?rlkey=koa5n1zyf8jm6i4lgu2hewvsx&dl=0)]

[[VirtualLoop Firmware 3.8](https://www.dropbox.com/scl/fi/4wwc64ikyemj3cxec7pc6/SAMD51_3.8.bin?rlkey=piafwqfs0macyg4v45py9ihme&dl=0)]

Download the firmware upgrade utility below and follow the instructions on the screen.

<img width="439" alt="Firmware Upgrader 1 0 0 0" src="https://github.com/user-attachments/assets/e1297ff2-eee5-4771-a81c-fac1eeb1fe7b" />

You'll need a USB micro cable like this:

<img width="200" alt="USB micro" src="https://github.com/user-attachments/assets/48831168-136d-4534-9b12-10fafdf12587"/>

Double-click the reset button:

<img width="300" alt="FW reset button" src="https://github.com/user-attachments/assets/8fbc860e-803a-4031-bbb5-e74d15a9ecbe"/>

## Contact<a name="contact"></a>
For assistance, please contact Brandon at brandon@apextalos.tech
