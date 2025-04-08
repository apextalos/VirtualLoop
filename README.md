# VirtualLoop

## BIU Rack card mounted
Power supplied by the cabinet backplane

<img height="250" alt="Rack_iso" src="https://github.com/user-attachments/assets/616049a2-af66-47e1-9e1f-77e0dc958a87" />
<img height="250" alt="Rack_side" src="https://github.com/user-attachments/assets/996f6862-f143-4e1a-8806-8dd1fbb3bdfd" />
<img height="250" alt="Rack_front" src="https://github.com/user-attachments/assets/eba9d5de-2109-4dc7-9f3c-2a36058491d5" />

## DIN rail mounted
Power supplied either by 12/24VDC on the front, or PoE 802.3af (consumes only ~1w)

2.1" wide x 6" deep x 4.1" high

This model also includes 8 relay outputs for TS1 integration

<img height="250" alt="Din_iso" src="https://github.com/user-attachments/assets/e606902a-25fe-4a86-87e3-f913a458dfb3" />
<img height="250" alt="Din_rear" src="https://github.com/user-attachments/assets/dd634314-a87f-4709-be97-53b4329bdefe" />
<img height="250" alt="Din_side" src="https://github.com/user-attachments/assets/fb541cc3-cbdc-4cb6-96dd-edceb44bea2e" />
<img height="250" alt="Din_front" src="https://github.com/user-attachments/assets/d0578019-3367-4cdd-824d-ca40280487d6" />

## Table of Contents
1. [Product information and datasheets](#datasheet)
2. [Getting started](#started)
2. [REST API](#restapi)
3. [Web Interface](#webui)
4. [Bosch Camera Script Generator Tool](#camerascript)
5. [GAIN Stack](#gain)
6. [Contact](#contact)

## Product information and datasheets<a name="datasheet"></a>
<img width="723" alt="overview" src="https://github.com/user-attachments/assets/fdd604a2-de2c-4213-968b-2eb035a5c74a" />

Presentation on Intersection Detection and ATSPM Data Collection: [[Intersection Video Detection.pptx](https://github.com/user-attachments/files/19509660/Intersection.Video.Detection.pptx)]

VirtualLoop AT-STE-02 Datasheet: [[VirtualLoop Datasheet.docx](https://github.com/user-attachments/files/19509669/VirtualLoop.Datasheet.docx)]

SDLC protocol reference: [[NEMA TS2-2003.pdf](https://www.dropbox.com/scl/fi/l3es29g8ugdyiyf4ybhre/NEMA-TS2-2003.pdf?rlkey=pro3as7zwp3ee7hn7jcgo2vp0&dl=0)]

## Getting started<a name="started"></a>
The device comes with the IP address 192.168.1.2 (or other as indicated by the OLED screen).  The web UI can be used to set a new IP address if needed.

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

## Bosch Camera Script Generator Tool<a name="camerascript"></a>
Generates the Bosch camera alarm task scripts for ease of use and to reduce errors.

1. Enter the IP address of the VirtualLoop device
2. Fill out the table with the mappings of cameras and rules to BIUs and detectors
3. Hit the "Generate" button and now you have each of the script blocks
4. Paste accordingly into your camera's Alarm Task Editors

<img width="1235" alt="mappings" src="https://github.com/user-attachments/assets/dc0dad7b-f46c-40ca-8c7d-655c6e1b3928" />

[[VirtualLoop Camera Script Generator Setup](https://www.dropbox.com/scl/fi/9izzetrmdh8i0pbwc6auz/CameraScriptGeneratorSetup.msi?rlkey=gnsn2c11pwpk1uz0p0apj3e3g&dl=0)]

## GAIN Stack - Reporting Dashboard<a name="gain"></a>
The GAIN (Grafana, ATSPM, Influx, NTP) stack can be deployed by running a "docker compose up" with this docker-compose.yml and is comprised of 4 main peices:

1. ATSPM - this is a container by Apex Talos which gather detection and phase data from either the VirtualLoop or the Controller's high-res data log.  The data is inserted into the time series database...
2. Influx - this is the time series database which will contain the ATSPM data and is queried by the Dashboard for charting and alerts...
3. Grafana - this is a very powerful dashboard designer which provides many ways to view data through a series of charts.  It also provides an alert engine for notification of when metrics reach thresholds
4. NTP - a simple NTP clock server for use if one does not already exist
   
```
services:
  influxdb:
    image: "influxdb:latest"
    container_name: at_influxdb
    volumes:
      - influxdb_data:/var/lib/influxdb
    ports:
      - "8086:8086"
    restart: always
  grafana:
    image: "grafana/grafana:latest"
    container_name: at_grafana
    volumes:
      - grafana_data:/var/lib/grafana
    ports:
      - "3000:3000"
    restart: always
  ntp:
    image: "cturra/ntp:latest"
    container_name: at_ntp
    ports:
      - "123:123/udp"
    restart: always
  sdl:
    image: "apextalosllc/sdl_multi_arch:buildx-latest"
    container_name: at_sdl
    ports:
      - "1337:1337"
    restart: always
    volumes:
      - /home/firstuser/GitHub/AT-STE/SDL/app.config:/opt/apextalos/sdl/app.config
volumes:
  influxdb_data:
    external: true
  grafana_data:
    external: true
```

The app.config file mapped by the SDL service looks like this:
```
device_ip_csv=192.168.14.34
device_period=0
db_url=http://192.168.14.35:8086/
db_token={DB TOKEN GOES HERE}
db_bucket=atspm
db_org=Apex Talos LLC
log_level=debug
perflog_device_json={"configs":[{"cutype":"YUNEX","host":"192.168.1.1","port":22,"user":"user","pass":"pass","keyfile":"","pathtodat":"/data"}]}
perflog_period=10000
```
## Contact<a name="contact"></a>
For assistance, please contact Brandon at brandon@apextalos.tech
