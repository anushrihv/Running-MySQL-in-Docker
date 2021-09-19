CONTENTS OF THIS FILE
---------------------

 * Introduction
 * Prerequisites
 * Pull MySQL Docker image
 * Steps to run your Database in Docker
 * Verify the running container
 * Connect to the MySQL server using a database management application like Sequel Pro

--------------------

INTRODUCTION
------------

To talk about the steps to run your MySQL Database in Docker

--------------------


PREREQUISITES 
-------------

An existing Docker installation

--------------------


PULL MYSQL DOCKER IMAGE
-----------------------
Pull the MySQL docker image:
  Replace the tag with a specific version or ```latest``` for the latest release.
  
  ```docker pull mysql/mysql-server:tag```
  
--------------------


STEPS TO RUN YOUR DATABASE IN DOCKER
------------------------------------
  ```
  docker run -d \
  --name=container_name \
  -v volume_name:/var/lib/mysql \
  --restart unless-stopped \
  -e MYSQL_ROOT_PASSWORD=password \
  -p 3306:3306 docker_image_name 
  ```
  * docker run: to run the container. To view all options available, run ```docker run --help``` in your terminal
  * -d: run the container in the background
  * --name: to set the name of the container
  * -v: to persist the data in Docker Volumes. This command creates a volume named mysqldata and mounts the path /var/lib/mysql within the container. You can also use bind-mount to mount a file or directory on the host machine into a container.
  * --restart: to set the restart policy. Docker provides restart policies to control whether your containers start automatically when they exit, or when Docker restarts.
  * -e: To set the environment variables within the scope of the container.
  * -p: To map the host machine's port to a container port

  Sample command:
  ```
  docker run -d \
  --name=mysql5.7 \
  -v mysqldata:/var/lib/mysql \
  --restart unless-stopped \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -p 3306:3306 mysql/mysql-server:5.7.16
  ```
  
--------------------

  
VERIFY THE RUNNING MySQL CONTAINER
----------------------------

Since we used the -d option to run the container in the background, we can use the following command to see the container logs:

```
docker logs container_name
```

To connect to the MySQL server from within the container:

```
docker exec -it mysql1 mysql -uroot -p{password}
SHOW DATABASES;
-- any commands that you want to run --
EXIT;
```

--------------------


CONNECT TO THE MySQL SERVER USING A DATABASE MANAGEMENT APPLICATION LIKE Sequel Pro
-----------------------------------------------------------------------------------

Parameters to use:

host: 127.0.0.1
username: root
password: 123456
port: 3306

Note: You might run into an error that says ```Host '172.18.0.1' is not allowed to connect to this MySQL server```. The reason is that, in MySQL each database user is defined with an IP address that the user can connect from. By default, the IP address is set to 127.0.0.1 for 'root' user. But since we are running the MySQL server on docker, we won't be connecting to the server from that IP address. 

The solution that worked for me:
```
docker exec -it container_name mysql -uroot -p123456

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

flush privileges;
```

You should be able to connect to the MySQL server now.





