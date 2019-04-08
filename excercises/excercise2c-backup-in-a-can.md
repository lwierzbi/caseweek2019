## Container backup one-liners

Docker containers can be attached to mount namespace of existing container, this allows neat single-liner backups 

1. Following will deploy container with busybox, attach to to hass container mount namespace, mount current directory inside this container on path '/backup', run tar command to compress /tmp/config into $(pwd)/backup/backup.tar; after all is done, backup container will be removed. All in single command B)

```console
docker run --rm --volumes-from hass -v $(pwd):/backup busybox tar cvf /backup/backup.tar /tmp/config
```
