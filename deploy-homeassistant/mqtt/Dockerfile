FROM node:latest
ADD ./mqtt-emulator /mqtt
ADD ./config /mqtt/config
RUN chmod a+x /mqtt/emulator.sh
WORKDIR /mqtt
CMD ["bash","./emulator.sh","--cf","/mqtt/config/config.json"]


