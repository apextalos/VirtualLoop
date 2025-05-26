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
