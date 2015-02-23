## Summary 
### Why yet another Docker image with php?
It is simple container with only one service inside - php-fpm. Follows one container - one service principle.

### What is inside?
- based on [phusion/baseimage](https://github.com/phusion/baseimage-docker).
- [php-fpm](http://packages.ubuntu.com/trusty/php5-fpm) from ubuntu 14.04 repository.
- xdebug with [remote_connect_back](http://xdebug.org/docs/remote).
- separate [pool config](https://github.com/r0b1n/dockerized-phpdev/blob/master/php-fpm/dev-pool.conf).

## How to run?
// TODO
