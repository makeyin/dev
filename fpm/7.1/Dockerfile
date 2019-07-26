FROM php:7.1.16-fpm-alpine3.4

COPY ssh /root/.ssh
WORKDIR /working

RUN echo 'http://mirrors.aliyun.com/alpine/v3.4/community/'>/etc/apk/repositories \
    && echo 'http://mirrors.aliyun.com/alpine/v3.4/main/'>>/etc/apk/repositories \
    && apk upgrade --update && apk add --no-cache --virtual build-dependencies $PHPIZE_DEPS \
    tzdata  openssh git postgresql-dev libxml2-dev libmcrypt libmcrypt-dev libmemcached-dev cyrus-sasl-dev autoconf  \
    && apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev libmemcached-dev \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd pdo pdo_mysql bcmath opcache \
    && pecl install memcached apcu redis \
    && docker-php-ext-enable memcached apcu redis \
    && apk del build-dependencies \
    && apk del tzdata \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/*  \
    && rm -rf /working/* \
    && rm -rf /usr/local/etc/php-fpm.d/* \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv ./composer.phar /bin/composer \
    && chmod 400 /root/.ssh/id_rsa \
    && rm -rf /var/cache/apk/* \
    && rm -rf /tmp/*

 
COPY ./php.ini /usr/local/etc/php/php.ini
COPY ./www.conf /usr/local/etc/php-fpm.d/www.conf
COPY phpcs /usr/bin/
COPY phpcbf /usr/bin/

 
