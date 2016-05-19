#
# php-app
#

FROM ubuntu:15.10
MAINTAINER Thomas BARTHELEMY <thomas.barthelemy@webridge.asia>

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install --no-install-recommends -qy apt-utils
RUN apt-get install --no-install-recommends -qy supervisor
RUN apt-get install --no-install-recommends -qy apache2
RUN apt-get install --no-install-recommends -qy php5 php5-curl php5-intl php5-pgsql php5-xdebug php5-apcu php5-dev
RUN apt-get install --no-install-recommends -qy git
RUN apt-get install --no-install-recommends -qy less vim-tiny
RUN apt-get install --no-install-recommends -qy apg
RUN apt-get install --no-install-recommends -qy sudo

# Supervisor
RUN mkdir -p /var/log/supervisor
ADD supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Apache2
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APP_DOCUMENT_ROOT /var/www
EXPOSE 80
ADD apache2/app.conf /etc/apache2/sites-available/000-default.conf
ADD apache2/apache2 /usr/local/sbin/apache2
RUN chmod 755 /usr/local/sbin/apache2
RUN a2enmod rewrite
RUN sed -i "s/^\\( *export \+LANG.*\\)/#\\1/" /etc/apache2/envvars

# PHP5
ENV PHP_INI ""
ENV XDEBUG_REMOTE_PORT ""
ADD php5/app.ini /etc/php5/apache2/conf.d/90-app.ini
ADD php5/app.ini /etc/php5/cli/conf.d/90-app.ini
ADD php5/timezone.sh /usr/local/sbin/php5-timezone.sh

# PHP application
ADD app/init /usr/local/sbin/app-init
RUN chmod 755 /usr/local/sbin/app-init

# System
ENV TZ ""
ENV LANG ""
ADD system/init /usr/local/sbin/system-init
RUN chmod 755 /usr/local/sbin/system-init
ADD system/locale.sh /usr/local/sbin/system-locale.sh
ADD system/timezone.sh /usr/local/sbin/system-timezone.sh

# Others
ENV HOME /root
WORKDIR /root
RUN mkdir /var/app
RUN echo "This file is a placeholder to expose /var/app directory after creating a new container from Kitematic." > /var/app/.placeholder
VOLUME /var/app

# Command
CMD ["/usr/local/sbin/system-init"]
