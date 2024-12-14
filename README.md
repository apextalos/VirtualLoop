# VirtualLoop by Apex Talos

## Rack card mounted
<img height="250" alt="iso" src="https://github.com/user-attachments/assets/b474068d-3669-458b-99df-0bd2ca2a9a7f" />
<img height="250" alt="left" src="https://github.com/user-attachments/assets/bafe1f9e-4b0f-471d-a2c6-e65a366fe996" />
<img height="250" alt="front" src="https://github.com/user-attachments/assets/9debfe4e-b31f-47bb-83b5-71367786c5e0" />

## DIN rail mounted
<img height="250" alt="iso" src="https://github.com/user-attachments/assets/58a3c2de-7595-4777-814c-cef77e05fb37" />
<img height="250" alt="front" src="https://github.com/user-attachments/assets/f4a21a4a-bde5-4201-9e49-760bfca61130" />
<img height="250" alt="rear" src="https://github.com/user-attachments/assets/3f938f77-f29d-4fa2-8562-610c10a8cbee" />

## Table of Contents
1. [Product information and datasheets](#datasheet)
2. [Getting started](#started)
2. [REST API](#restapi)
3. [Web Interface](#webui)
4. [Camera Script Generator Tool](#camerascript)
5. [GAIN Stack](#gain)
6. [Contact](#contact)

## Product information and datasheets<a name="datasheet"></a>
Presentation on Intersection Detection and ATSPM Data Collection: [[Intersection Video Detection.pptx](https://www.dropbox.com/scl/fi/hmjmytsoohgrm2d3pi5ze/Intersection-Video-Detection.pptx?rlkey=ou01154nqmh2z1plagp346tsq&dl=0)]

VirtualLoop AT-STE-02 Datasheet: [[AT-STE-02 Datasheet.pdf](https://www.dropbox.com/scl/fi/lmun6xd79sjraupx2icg0/AT-STE-02-Datasheet.pdf?rlkey=wtu0g6mmazsuxcskwamrxzm6k&dl=0)]

SDLC protocol reference: [[NEMA TS2-2003.pdf](https://www.dropbox.com/scl/fi/l3es29g8ugdyiyf4ybhre/NEMA-TS2-2003.pdf?rlkey=pro3as7zwp3ee7hn7jcgo2vp0&dl=0)]

## Getting started<a name="started"></a>
The device comes with the IP address 192.168.1.2 as indicated by the OLED screen.  The web UI can be used to set a new IP address if needed.

Holding the front button for 5 seconds or longer will factory reset the device.

## REST API<a name="restapi"></a>
Supports placing calls on specific detector #, as well as other features

PostMan API tools: [[PostMan](https://documenter.getpostman.com/view/2915446/2sAXqp8PEb)]

## Web Interface<a name="webui"></a>
Status page

<kbd><img width="500" alt="image" src="https://github.com/user-attachments/assets/c1351d62-70e9-44c8-9e33-c10a90b58fca" /></kbd>

Detector selection page

<kbd><img width="500" alt="image" src="https://github.com/user-attachments/assets/9f2497f4-8593-4c69-83f9-fb4388da6c29" /></kbd>

Network configuration page

<kbd><img width="500" alt="image" src="https://github.com/user-attachments/assets/0df74542-862e-4e7c-8435-20d383dabeba" /></kbd>

## Camera Script Generator Tool<a name="camerascript"></a>
Generates the Bosch camera alarm scripts for ease of use and to reduce errors.

1. Enter the IP address of the VirtualLoop device
2. Fill out the table with the mappings of cameras and rules to BIUs and detectors
3. Hit the "Generate" button and now you have each of the script blocks
4. Paste accordingly into your camera's Alarm Task Editors
<img width="524" alt="1734024433648blob" src="https://github.com/user-attachments/assets/bb9cfa4f-0c57-435f-859e-aada77d360c4" />

[VirtualLoop Camera Script Generator 1.0.0.3.zip](https://github.com/user-attachments/files/18127058/VirtualLoop.Camera.Script.Generator.1.0.0.3.zip)

## GAIN Stack<a name="gain"></a>
The GAIN (Grafana, ATSPM, Influx, NTP) stack can be deployed by running a "docker compose up" with this docker-compose.yml
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
