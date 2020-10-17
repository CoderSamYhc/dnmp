FROM php:7.3-fpm-alpine

ENV SWOOLE_VERSION=4.5.3
ENV PHP_REDIS=5.3.1

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# Libs
RUN apk update
RUN apk add --no-cache curl wget vim gcc g++ autoconf make \
    openssl-dev \
    librdkafka-dev \
    libmcrypt-dev \
    libxml2-dev \
    icu-dev \
    libzip-dev \
    libpng-dev

RUN echo "Asia/Shanghai" > /etc/timezone

# Extensions
RUN docker-php-ext-install gd bcmath opcache mysqli pdo pdo_mysql sockets zip pcntl


RUN echo "Asia/Shanghai" > /etc/timezone

# Composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer self-update --clean-backups \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/


# Extension redis bcmath mongodb rdkafka
RUN pecl install redis mcrypt mongodb rdkafka \
        && docker-php-ext-enable redis mcrypt mongodb rdkafka

# Redis extension
RUN wget http://pecl.php.net/get/redis-${PHP_REDIS}.tgz -O /tmp/redis.tar.tgz \
    && pecl install /tmp/redis.tar.tgz \
    && rm -rf /tmp/redis.tar.tgz \
    && docker-php-ext-enable redis

# XDEBUG extension
# RUN wget https://xdebug.org/files/xdebug-${PHP_XDEBUG}.tgz -O /tmp/xdebug.tar.tgz \
#     && pecl install /tmp/xdebug.tar.tgz \
#     && rm -rf /tmp/xdebug.tar.tgz \
#     && docker-php-ext-enable xdebug

# Hiredis
#RUN wget https://github.com/redis/hiredis/archive/v${HIREDIS_VERSION}.tar.gz -O hiredis.tar.gz \
#    && mkdir -p hiredis \
#    && tar -xf hiredis.tar.gz -C hiredis --strip-components=1 \
#    && rm hiredis.tar.gz \
#    && ( \
#        cd hiredis \
#        && make -j$(nproc) \
#        && make install \
#        && ldconfig \
#    ) \
#    && rm -r hiredis

# Swoole extension
RUN wget https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz -O swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    && ( \
    cd swoole \
        && phpize \
        && ./configure --enable-async-redis --enable-mysqlnd --enable-openssl --enable-http2 \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole

RUN php -m

EXPOSE 9500 9501 9502 9503 9504