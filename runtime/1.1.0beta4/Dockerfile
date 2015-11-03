################################################################################
# Dockerfile for appserver-runtime 1.1.0beta4
################################################################################

# base image
FROM debian:jessie

# author
MAINTAINER Johann Zelger <jz@appserver.io>

# define runtime version
ENV APPSERVER_RUNTIME_BUILD_VERSION 1.1.0-37+beta4

# install packages
RUN apt-get update && \
	DEBIAN_FRONTEND=noninteractive apt-get install wget php5 php5-curl gdb -y
	
# install composer
RUN cd /usr/local/bin && php -r "readfile('https://getcomposer.org/installer');" | php
RUN ln -s /usr/local/bin/composer.phar /usr/local/bin/composer	

# download runtime in specific version
RUN wget -O /tmp/appserver-runtime.deb \
	http://builds.appserver.io/linux/debian/8/appserver-runtime_${APPSERVER_RUNTIME_BUILD_VERSION}~deb8_amd64.deb
	
# install runtime
RUN dpkg -i /tmp/appserver-runtime.deb; exit 0
RUN apt-get install -yf
RUN rm -f /tmp/appserver-runtime.deb

# forward request and error logs to docker log collector
RUN ln -sf /dev/stderr /opt/appserver/var/log/php_errors.log

# define mountable volumes
VOLUME /root/appserver

# define working directory
WORKDIR /root/appserver

# expose ports
EXPOSE 9080 9443

# define default command
CMD ["/opt/appserver/bin/php", "server.php"]