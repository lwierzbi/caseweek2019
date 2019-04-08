FROM python:latest
ENV config_dir=/hasscfg
LABEL Description="This image contains home-assistant server"
RUN python3 -m pip install homeassistant==0.90.2 \
    sqlalchemy==1.2.18 \
    aiohttp_cors==0.7.0 \
    netdisco==2.5.0 \
    xmltodict==0.11.0 \
    zeroconf==0.21.3 \
    hass-nabucasa==0.11 \
    home-assistant-frontend==20190321.0 \
    hbmqtt==0.9.4
RUN python3 -m pip install pymysql
RUN python3 -m pip install paho-mqtt==1.4.0
RUN mkdir -p $config_dir
ADD ./config/configuration.yaml $config_dir/
CMD hass --open-ui --config $config_dir
#CMD hass --open-ui
EXPOSE 8123
