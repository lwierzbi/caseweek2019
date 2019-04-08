## Docker images - create from containers

1. Run container from image python:latest in interactive mode

```console
docker run -ti --name my-hass python:latest /bin/bash
```

2. While inside container, install homeassistant package, then exit container

```console
python3 -m pip install homeassistant==0.90.2 \
sqlalchemy==1.2.18 \
aiohttp_cors==0.7.0 \
netdisco==2.5.0 \
xmltodict==0.11.0 \
zeroconf==0.21.3 \
hass-nabucasa==0.11 \
home-assistant-frontend==20190321.0 \
hbmqtt==0.9.4

...
exit
```
3. Container is now in exited state, we can convert it into image

```console
docker commit my-hass my-hass:v1
```

4. Check that new image has been created and is available locally
```console
docker images
```
