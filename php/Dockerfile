FROM php:7.4.0-cli-alpine

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN php -v

# 安装swoole
ADD ./php-exts/swoole-4.5.5.tgz /tmp/
RUN cd /tmp/swoole-4.5.5 && phpize && ./configure && make && make install
RUN docker-php-ext-enable swoole

# 安装redis
ADD ./php-exts/redis-4.2.0.tgz /tmp/
RUN cd /tmp/redis-4.2.0 && phpize && ./configure && make && make install

RUN docker-php-ext-enable redis
# 开启pdo
RUN docker-php-ext-install pdo_mysql

# 安装composer
ADD ./php-exts/composer.phar /tmp/
RUN set -ex \
    && chmod u+x /tmp/composer.phar \
    && mv /tmp/composer.phar /usr/local/bin/composer \
    # show php version and extensions
    && php -v \
    && php -m \
    && php --ri swoole \
    && php --ri redis 

WORKDIR /opt/www

EXPOSE 9601
