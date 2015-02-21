FROM phusion/baseimage


# Simple installing and configuring
RUN apt-get update && \
    apt-get -y install nginx && \
    
    echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# TODO: add site config.
ADD dev.server /etc/nginx/sites-available/
RUN ln -sf /etc/nginx/sites-available/dev.server /etc/nginx/sites-enabled/


# TODO: add data volumes.


# Expose ports.
EXPOSE 8080
EXPOSE 443

# Define default command.
CMD ["nginx"]
