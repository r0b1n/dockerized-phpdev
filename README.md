# dockerized-phpdev
Docker images for building php development environment

#### Building images

```bash
docker build -t nginx-dev nginx # build nginx image with tag 'nginx-dev' from 'nginx' directory

docker build -t php-fpm-dev php-fpm # build php-fpm image with tag 'php-fpm-dev' from 'php-fpm' directory


# optional build mysql image
```

#### Running containers from images
In this examples assumed what we have folder `/export/sites/f8d3d1/` with source code and it will be document root for server.
First php-fpm container
```bash
sudo docker run -d -i \ 
	-v /export/sites/f8d3d1/:/srv \ # forward host directory inside of container (it may be accesable by developer from outside)
	# --link dev-mysql-f8d3d1:database \ # optional we can link additional container with other services to our fpm  (mysql, redis, mongo, etc).
	--name dev-fpm-f8d3d1 # unique container name
	php-fpm-dev
```

then nginx container with link to php-fpm
```bash
sudo docker run -d -i \ 
	-v /export/sites/f8d3d1/:/srv \ # forwarding same directory as for php-fpm container.
	--link dev-fpm-f8d3d1:fpm \ # link with php-fpm container. NOTE: inside of nginx controller 'dev-fpm-f8d3d1' will be accessable by 'fpm' hostname!
	-p 8080:80 \ # expose 80 port to 8080 host port. will be available using http://host_ip:8080. TODO: add reverse proxy for containers.
	--name dev-nginx-f8d3d1 \ # unique container name
	nginx-dev # based on this image
```
