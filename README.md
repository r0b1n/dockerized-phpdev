# dockerized-phpdev
Docker images for building php development environment.

#### Running environment using docker-compose (ex. Fig)
[docker-compose](https://github.com/docker/compose) is a great tool for running bunch of containers in one command. `docker-compose` uses `docker-compose.yml` file with defined structure of containers bundle.
d
In example we going to run:
* `db` - database container with mysql server. Based on [official mysql docker image](https://registry.hub.docker.com/_/mysql/).
* `php` - php-fpm container with linked database container, so our php code may connect to mysql server.
* `web` - nginx container with linked php container, nginx uses php-fpm as FastCGI backend.

You may change mysql credentials, volumes settings, port binding.
Then run environment in simple way with `docker-compose`:
```bash
$ docker-compose up -d
```
After downloading and building images site will be available at http://$(boot2docker ip):8888/. In my case:
```bash
$ echo "http://$(boot2docker ip):8888/"
http://192.168.59.103:8888/
```

###### Interact with php container.
To run console commands such as composer, migrations, tests we need to run bash inside php container, it is simple, first we heed to find container name:
```bash
$ docker-compose ps
    Name               Command          State               Ports
-----------------------------------------------------------------------------
phpdev_db_1    /entrypoint.sh mysqld   Up      3306/tcp
phpdev_php_1   php5-fpm -F             Up      9000/tcp
phpdev_web_1   nginx                   Up      443/tcp, 0.0.0.0:8888->8080/tcp
```
In this case name of php container is `phpdev_php_1`, to get bash access:
```bash
$ docker exec -it phpdev_php_1 bash
root@85cbd528a0f5:/#
```
Means run bash inside `phpdev_php_1` container in interactive mode. And we are in bash. Note: composer is [pre-installed](https://github.com/r0b1n/dockerized-phpdev/blob/master/php-fpm/Dockerfile#L23:L24) in php-fpm image.


#### Building images manually
In this case we going to build new docker images manually. After building we can use images to run containers in manual mode or by `docker-compose`.
```bash
docker build -t nginx-dev nginx # build nginx image with tag 'nginx-dev' from 'nginx' directory
docker build -t php-fpm-dev php-fpm # build php-fpm image with tag 'php-fpm-dev' from 'php-fpm' directory
```

#### Running containers from images
In this examples assumed what we have folder `/export/sites/f8d3d1/` with source code and `/export/sites/f8d3d1/public` will be document root for server (mapped to `/srv/public`).
First php-fpm container
```bash
sudo docker run -d -i \ 
	-v /export/sites/f8d3d1/:/srv \ # forward host directory inside of container (it may be available by developer from outside)
	# --link dev-mysql-f8d3d1:database \ # optional we can link additional container with other services to our fpm  (mysql, redis, mongo, etc).
	--name dev-fpm-f8d3d1 # unique container name
	php-fpm-dev # image name (assumed image built in previous step) or use r0b1n/dockerized-phpdev-fpm
```

then nginx container with link to php-fpm
```bash
sudo docker run -d -i \ 
	-v /export/sites/f8d3d1/:/srv \ # forwarding same directory as for php-fpm container.
	--link dev-fpm-f8d3d1:php \ # link with php-fpm container. NOTE: inside of nginx controller 'dev-fpm-f8d3d1' will be available by 'php' hostname!
	-p 8080:80 \ # expose 80 port to 8080 host port. will be available using http://host_ip:8080. TODO: add reverse proxy for containers.
	--name dev-nginx-f8d3d1 \ # unique container name
	nginx-dev # image name (assumed image built in prevoius step) or use r0b1n/dockerized-phpdev-nginx
```
