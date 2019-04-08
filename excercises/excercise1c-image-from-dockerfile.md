## Docker images - create from dockerfile 

1. create 'homeassistant' directory with following files:

```
homeassistant
    Dockerfile
    configuration.yaml
```

2. We are now going to use homeassistant container to build our own, custom one

Create following Dockerfile

```dockerfile
FROM my-hass:v1
ENV config_dir=/etc
LABEL Description="This image contains home-assistant server"
RUN mkdir -p $config_dir
ADD configuration.yaml $config_dir/
CMD hass --open-ui --config $config_dir
#CMD hass --open-ui
EXPOSE 8123
```

3. Run build process
```console
docker build -t my-hass:v2 ./homeassistant
```

4. Test newly created image
```console
docker run --name my-hass -d my-hass:v2
```

* TASK 
Prepare dockerfile that will include build steps from version v1 and v2

* TASK 
Try to minimize image size (TIP: check for alternative base images, we probably don't need majority of stuff stored in our container)