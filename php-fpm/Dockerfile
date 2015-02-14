FROM phusion/baseimage


# Install packages. TODO: review packages
RUN apt-get update && \
    apt-get -y install \
    curl \
    php5-fpm \
    php5-mysql \
    php5-imagick \
    php5-imap \
    php5-mcrypt \
    php5-curl \
    php5-cli \
    php5-gd \
    php5-common \
    php-pear \
    php5-json \
    php5-xdebug 

# Install composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ && \
    mv /usr/bin/composer.phar /usr/bin/composer 

# Clean image 
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure php5-fpm
# TODO: configure Xdebug for remote debug.
RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php5/fpm/php.ini && \
    sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/fpm/php.ini && \
    sed -i "s/display_errors = Off/display_errors = stderr/" /etc/php5/fpm/php.ini && \
    sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 30M/" /etc/php5/fpm/php.ini && \
    sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php5/fpm/php-fpm.conf && \
    sed -i '/^listen = /clisten = 9000' /etc/php5/fpm/pool.d/www.conf && \
    sed -i '/^listen.allowed_clients/c;listen.allowed_clients =' /etc/php5/fpm/pool.d/www.conf && \
    sed -i '/^;catch_workers_output/ccatch_workers_output = yes' /etc/php5/fpm/pool.d/www.conf
    #sed -i '/^;env\[TEMP\] = .*/aenv[DB_PORT_3306_TCP_ADDR] = $DB_PORT_3306_TCP_ADDR' /etc/php5/fpm/pool.d/www.conf

ADD 30-xdebug.ini /etc/php5/fpm/conf.d/

# TODO: add data volumes 

EXPOSE 9000

ENTRYPOINT ["php5-fpm","-F"]
