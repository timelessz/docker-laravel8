FROM php:8.0.10-fpm-buster
# 参考：https://zhuanlan.zhihu.com/p/381019429
# https://registry.hub.docker.com/_/php

MAINTAINER timelesszhuang
MAINTAINER 834916321@qq.com
MAINTAINER 2021/9/29
MAINTAINER v1.0.0

EXPOSE 9000
WORKDIR /var/www/html

# source extract
RUN docker-php-source extract

# env
RUN apt-get update \
    && apt-get -y install wget libc-client-dev libkrb5-dev libzip-dev zip unzip

# package build
RUN mkdir /packages

# 安装mcrprt
RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv

# mcrypt  redis mongodb
RUN pecl install mcrypt-1.0.4 \
    && pecl install redis-5.3.4 \
    && docker-php-ext-enable mcrypt redis


# mongodb  安装失败
#RUN pecl install mongodb-1.10.0 \
#    && docker-php-ext-enable mongodb


RUN docker-php-ext-configure gd --with-jpeg=/usr/include --with-freetype=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-enable gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-enable pdo_mysql


# imap extension
RUN docker-php-ext-configure imap \
    --with-kerberos \
    --with-imap-ssl \
    && docker-php-ext-install imap \
    && docker-php-ext-enable imap

# zlib extension
RUN mv /usr/src/php/ext/zlib/config0.m4 /usr/src/php/ext/zlib/config.m4 \
    && docker-php-ext-install zlib \
    && docker-php-ext-enable zlib



# 安装 xcache  安装失败
# RUN curl -fsSL 'https://github.com/lighttpd/xcache/archive/refs/tags/3.2.0.tar.gz' -o xcache.tar.gz \
#    && mkdir -p xcache \
#    && tar -xf xcache.tar.gz -C xcache --strip-components=1 \
#    && rm xcache.tar.gz \
#    && ( \
#        cd xcache \
#        && phpize \
#        && ./configure --enable-xcache \
#        && make -j$(nproc) \
#        && make install \
#    ) \
#    && rm -r xcache \
#    && docker-php-ext-enable xcache


# confiugure
RUN cd /usr/local/etc/php \
    && cp php.ini-production php.ini \
    && sed -i 's/display_errors\s*=.*/display_errors = Off/' php.ini \
    && sed -i 's/error_reporting\s*=.*/error_reporting = E_ALL \& ~E_NOTICE \& ~E_STRICT \& ~E_DEPRECATED/' php.ini \
    && sed -i 's/;error_log\s*=\s*php_errors\.log/error_log = \/var\/log\/php_errors.log/' php.ini \
    && sed -i 's/;date\.timezone\s*=.*/date.timezone = Asia\/Shanghai/' php.ini


# package delete
RUN rm -rf /packages

# source delete
RUN docker-php-source delete

# 安装composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"\
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

