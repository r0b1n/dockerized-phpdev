## dockerized-phpdev/nginx

Simple nginx image.

### What is inside?
- based on [phusion/baseimage](https://github.com/phusion/baseimage-docker).
- [nginx](http://packages.ubuntu.com/trusty/nginx) form Ubuntu 14.04 repository.
- Separate [server config](https://github.com/r0b1n/dockerized-phpdev/blob/master/nginx/dev.server) on `8080` port.
- Expects php-fpm on `php:9000`. `php` - linked container. See [php-fpm](https://github.com/r0b1n/dockerized-phpdev/tree/master/php-fpm)

## How to run

Check [here](https://github.com/r0b1n/dockerized-phpdev)