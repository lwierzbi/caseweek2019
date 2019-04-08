## Data persistency - docker volumes

To ensure data persistency regardless of container lifecycle we can use volumes (named or unnamed).
Docker volumes are managed by docker engine, and with their own life cycle. Volume storage backend
depends on volume driver. By default it is local file system, but it can be distributed file system,
NFS, or any other driver implementing docker's volume API

1. Let's deploy container once again, this time with volume

```console
docker run --name hass --volume my-hass-vol:/tmp/config:rw -d my-hass:v2 hass --open-ui --config /tmp/config
```

If volume that we attach (in our case my-hass-vol) does not exist, default volume is created (with driver local)

```console
docker volume ls
```

2. We can inspect volume metadata

```console
docker inspect my-hass-vol
```
3. Create an user account, you can see that now ~/my-hass-config contains also database file and other files created during first launch

4. Now we will kill and remove container, and then run it again to check whether our settings were safely stored

```console
docker kill hass
docker rm hass
docker run --name hass --volume my-hass-vol:/tmp/config:rw -d my-hass:v2 hass --open-ui --config /tmp/config
```
