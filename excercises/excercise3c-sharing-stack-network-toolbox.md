# Sharing network namespace between containers

Very often, when debuggin our application, we dont's have all the tools we need in investigated container(s).
Sometimes we can simply get inside container and install what we need for analysis, but there is better way:
prepare toolbax container, with all the things you need for investigation. In our case, this will be network toolbox
build on Alpine distribution (due to its small footprint).

Let's build the container using following dockerfile:
```docker
FROM alpine:3.9

RUN apk update \
    && apk add net-tools \
    && apk add curl \
    && apk add tcpdump \
    && apk add iputils
```

Now, attach it to running container by adding following oprion to run command: 
```console 
--network container:<target container name> 
```

Run several commands, like ifconfig or tcpdump, inside toolbox container, detach it from one container and attach to another.


