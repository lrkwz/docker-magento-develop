FROM php:5.6-fpm

MAINTAINER Luca Orlandi <luca.orlandi@gmail.com>

# install the PHP extensions we need
RUN apt-get update && apt-get install -y libpng12-dev libjpeg-dev libmcrypt-dev libcurl4-openssl-dev libicu-dev \
 && rm -rf /var/lib/apt/lists/* \
 && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
 && docker-php-ext-install gd mysql opcache curl mcrypt intl pdo pdo_mysql \
 && pecl install xdebug

ADD magento.ini /etc/php5/fpm/conf.d/
ADD magento.ini /etc/php5/cli/conf.d/
ADD magento.pool.conf /etc/php5/fpm/pool.d/
ADD 20-xdebug.ini  /etc/php5/fpm/conf.d/
ADD 20-xdebug.ini  /etc/php5/cli/conf.d/

RUN usermod -u 1000 www-data

RUN apt-get update && apt-get install -y msmtp
ADD msmtprc /etc/.msmtp_php
RUN chown www-data. /etc/.msmtp_php && chmod 600 /etc/.msmtp_php

ADD crontab /etc/cron.d/magento
RUN chmod 0644 /etc/cron.d/magento

VOLUME /var/www/magento
RUN mkdir /var/www/magento/var /var/www/magento/media && chown -Rf www-data. /var/www/magento/var /var/www/magento/media

COPY local.xml /
COPY startup.sh /

CMD ["/startup.sh"]

EXPOSE 9000
