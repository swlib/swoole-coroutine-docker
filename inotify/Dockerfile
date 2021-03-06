FROM php:latest

MAINTAINER twosee <twose@qq.com>

# install modules : GD iconv
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        openssl \
        libssh-dev \
        libnghttp2-dev \
        libhiredis-dev \
    && docker-php-ext-install iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

# install php pdo_mysql opcache
# WARNING: Disable opcache-cli if you run you php
RUN docker-php-ext-install pdo_mysql mysqli iconv mbstring json opcache && \
    echo "opcache.enable_cli=0" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

# install inotify
RUN apt-get install -y inotify-tools

# install composer
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -sS https://getcomposer.org/installer | php && \
mv composer.phar /usr/local/bin/composer && \
composer self-update --clean-backups && \
apt-get install -y zip unzip

# install node npm
RUN apt-get install -my wget gnupg && \
curl -sL https://deb.nodesource.com/setup_9.x | bash - && \
apt-get install -y nodejs && \
npm install -g cnpm --registry=https://registry.npm.taobao.org

# install git
RUN apt-get install -y git

# set China timezone
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo 'Asia/Shanghai' > /etc/timezone && \
    echo "[Date]\ndate.timezone=Asia/Shanghai" > /usr/local/etc/php/conf.d/timezone.ini