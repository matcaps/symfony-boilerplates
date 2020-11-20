FROM php:7.4-apache

RUN apt -y update

# Install Apt dependencies
RUN apt -y install locales locales-all libmagickwand-dev libzip-dev subversion git gettext libonig-dev

# Install Imagick
RUN pecl install imagick
RUN echo "extension=imagick.so" > /usr/local/etc/php/conf.d/imagick.ini

# Install composer
RUN curl --silent --fail --location --retry 3 --output /tmp/installer.php --url https://getcomposer.org/installer
RUN php /tmp/installer.php --no-ansi --install-dir=/usr/bin --filename=composer --version=1.8.0
RUN composer --ansi --version --no-interaction

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/xdebug.ini



# PHP Extensions
RUN pecl install redis
RUN docker-php-ext-enable redis xdebug
RUN docker-php-ext-install -j$(nproc) mbstring gd pdo_mysql gettext bcmath zip soap calendar opcache

#-

# Set Apache Defaults
ENV APACHE_DOCUMENT_ROOT /app/web
ENV APACHE_SERVER_ADMIN spackala@icloud.com

# Set Default Workingdir
WORKDIR /app

# Enable Apache Rewrite Engine
RUN a2enmod rewrite

# Copy Phpinfo to Webroot
#COPY phpinfo.php /app/web/public/index.php

# Copy Default Apache Config
COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf

# Install Xdebug
#RUN pecl install xdebug

#RUN echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
#    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
#    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
#    && echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/xdebug.ini

# Expose Port 80
EXPOSE 80
