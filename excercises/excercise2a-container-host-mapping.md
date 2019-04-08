## mapping host directory for easy development and experimentation

When experimenting with new service configuration, it is often convieneint to have part of container's filesystem mapped easily accessible in host path. This way you can alter configuration and see effects immediately after container restarts

1. Prepare directory with configuration file (can be found in resources)

```console
mkdir ~/my-hass-dir
cp configuration.yaml ~/my-hass-dir
```
2. When starting a container, mount created directory inside 

```console
docker run --name hass \
--volume ~/my-hass-dir:/tmp/config:rw \
-d my-hass:v2 hass --open-ui --config /tmp/config
```

3. Check container logs if application started without problems, look for IP address of container and use web browser to access service console

```console
docker logs -f hass
```
Look for line like:

```console
2019-04-02 10:13:45 INFO (MainThread) [homeassistant.components.discovery] Unknown service discovered: home_assistant {'host': '172.17.0.11', 'port': 8123, 'hostname': 'Home._home-assistant._tcp.local.', 'properties': {'version': '0.90.2', 'base_url': 'http://172.17.0.11:8123', 'requires_api_password': True}}
```

4. Create an user account, you can see that now ~/my-hass-config contains also database file and other files created during first launch

5. Now we will kill and remove container, and then run it again to check whether our settings were safely stored

```console
docker kill hass
docker rm hass
docker run --name hass --volume ~/my-hass-dir:/tmp/config:rw -d my-hass:v2 hass --open-ui --config /tmp/config
```

# Running host application in a container
## Host mapping gives access to host filesystem resources, like device drivers, unix sockets, etc...

1. Cadvisor is simple application for container monitoring (it also exports container metrics for external monitoring components)

Following instructions on github site https://github.com/google/cadvisor we can run it inside container

```console
sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```
Now, check http://localhost:8080 in your browser.
You can also see that metrics are available at endpoint localhost:8080/metrics - using web browser or 

```console
curl http://localhost:8080/metrics
```


