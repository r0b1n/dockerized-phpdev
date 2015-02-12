FROM phusion/baseimage


# Simple installing and configuring
RUN apt-get update && \
    apt-get -y install nginx && \
    
    echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# TODO: add site config.

# TODO: add data volumes.


# Expose ports.
EXPOSE 80
EXPOSE 443

# Define default command.
CMD ["nginx"]
