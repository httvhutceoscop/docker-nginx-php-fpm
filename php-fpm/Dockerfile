FROM php:7.3.0-fpm

RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        vim \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpq-dev \
        libzip-dev  \
        libicu-dev  \
        libsodium-dev  \
        && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip pdo_pgsql pgsql \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install -j$(nproc) sodium

RUN pecl channel-update pecl.php.net && pecl install mcrypt

COPY symfony.pool.conf  /usr/local/etc/
COPY php.ini /usr/local/etc/php/

RUN git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis
RUN docker-php-ext-install redis

# install php composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

RUN usermod -u 1000 www-data

# install Application
#USER www-data
#WORKDIR /var/www/html
#RUN composer install

EXPOSE 9000
CMD ["php-fpm", "-F"]
