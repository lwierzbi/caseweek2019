#Connecting containers with bridge network

Home assistant by default stores all the data in SQLite database, which is fine in most of the cases, 
however for various reasons me might want to use external database backend. Fortunatelly, application
can be configured to use external datastore.

1. Let's create deploy application and database in separate containers, communicationg through dedicated network.

We can create default bridge netork using command:

```console
docker network create hass_network
```
You can list all networks with command
```console
docker network ls
```
and get more details about each of them with 
```console
docker network inspect <network name>
```

2. For database backend, homeassistant offers choice of several SQL databases, like for example MariaDB.
We set root password using environment variable passed to the container, and attach container to 
recently created network:

* TASK: 

We would like to deploy database with storage separated from the container. Please modify below command accordingly. 

```console
docker run -d --name mariadb -e MYSQL_ROOT_PASSWORD=rootsecret --network hass_network mariadb
```
3. Next, we have to prepare our database for application, by creating new database and user
Run mysql client, connect to the database using root credentials, and execute following:

```sql
create database myhome;
create user 'hass' identified by 'secret';
grant all on myhome.* to 'hass';
```

4. Finally, we have to configure homeassistant to use external database, we need to modify configuration file by adding mariadb recorder

```yaml
recorder:
        db_url: mysql+pymysql://hass:secret@mariadb/myhome?charset=utf8
```

Now we can attach hass container to new network

```console
docker network connect hass_network hass
```
And restart homeassistent container to apply changes in configuration

```console
docker restart hass
```
Now, homeassistant should already use our database, please check if application has modified the database.


