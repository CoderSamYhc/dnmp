FROM php:7.2-fpm-alpine

ENV SWOOLE_VERSION=4.5.3
ENV PHP_REDIS=5.3.1

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN echo "Asia/Shanghai" > /etc/timezone

# update
RUN set -ex \
    && apk update \
    && apk add --no-cache libstdc++ wget openssl bash \
    libmcrypt-dev libzip-dev libpng-dev libc-dev zlib-dev librdkafka-dev libmemcached-dev cyrus-sasl-dev

RUN apk add --no-cache --virtual .build-deps autoconf automake make g++ gcc \
    libtool dpkg-dev dpkg pkgconf file re2c pcre-dev php7-dev php7-pear openssl-dev \

    # 安装php常用扩展
    && docker-php-ext-install gd bcmath opcache mysqli pdo pdo_mysql sockets zip pcntl \

    # Extension memcached redis bcmath mongodb rdkafka
    && pecl install memcached redis mcrypt mongodb rdkafka \
    && docker-php-ext-enable memcached redis mcrypt mongodb rdkafka \

    # 安装 Composer
    && wget https://mirrors.cloud.tencent.com/composer/composer.phar \
    && mv composer.phar  /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \

    # 安装 Swoole
#    && wget http://pecl.php.net/get/swoole-${SWOOLE_VERSION}.tgz -O swoole.tar.gz \
#    && mkdir -p swoole \
#    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
#    && rm swoole.tar.gz \
#    && ( \
#        cd swoole \
#        && phpize \
#        && ./configure --enable-mysqlnd --enable-openssl \
#        && make -s -j$(nproc) && make install \
#    ) \
#    && rm -r swoole \
#    && docker-php-ext-enable swoole \

    # 安装xlswriter
    && curl -fsSL "https://pecl.php.net/get/xlswriter-1.3.4.1.tgz" -o xlswriter.tgz \
    && mkdir -p xlswriter \
    && tar -xf xlswriter.tgz -C xlswriter --strip-components=1 \
    && rm xlswriter.tgz \
    && ( \
        cd xlswriter \
        && phpize \
        && ./configure --enable-reader \
        && make && make install \
    ) \
    && rm -rf xlswriter \
    && docker-php-ext-enable xlswriter \

    # 删除系统扩展
    && apk del .build-deps \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    && php -m \

    # 删除/vaw/www/html目录
    && rm -rf /var/www/html