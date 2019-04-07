## Docker images - running images downloaded from DockerHub

1. Pull the latest Redis database image

```console
docker pull redis:latest
```

2. Run Redis database in container, for now we don't care about configuration and persistance

```console
docker run --name redis-test -d redis
```
3. Let's check if our container is running

```console
docker ps
```
```console
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                  NAMES
66276e8a533d        redis                  "docker-entrypoint.s…"   36 minutes ago      Up 36 minutes       6379/tcp               my-redis
```
4. Now, run redis-cli inside database container and play with database a bit...
```console
docker exec -ti my-redis redis-cli

127.0.0.1:6379> 
127.0.0.1:6379> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
127.0.0.1:6379> GEODIST Sicily Palermo Catania
"166274.1516"
127.0.0.1:6379> GEORADIUS Sicily 15 37 100 km
1) "Catania"
127.0.0.1:6379> GEORADIUS Sicily 15 37 200 km
1) "Palermo"
2) "Catania"
```
