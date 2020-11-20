FROM php:7.4.0-cli-alpine
LABEL maintainer="Lixs" version="1.0" license="MIT" app.name="Lixs"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN echo -e 'https://mirrors.aliyun.com/alpine/v3.6/main/\nhttps://mirrors.aliyun.com/alpine/v3.6/community/' > /etc/apk/repositories \
    && apk update \
    && apk add make \
    && apk add autoconf \
    && apk add gcc \
    && apk add gcc g++

# RUN cd / && mkdir tmp

# 安装 swoole
ADD ./php-exts/swoole-4.5.5.tgz /tmp/
RUN cd /tmp/swoole-4.5.5 && phpize && ./configure && make && make install
RUN docker-php-ext-enable swoole

# 安装 redis
ADD ./php-exts/redis-4.2.0.tgz /tmp/
RUN tar -zxf /tmp/redis-4.2.0.tgz && cd /tmp/redis-4.2.0 && phpize && ./configure && make && make install
RUN docker-php-ext-enable redis

# 安装 pdo_mysql
RUN docker-php-ext-install pdo_mysql

# 安装composer
COPY ./php-exts/composer.phar /usr/local/bin/composer
RUN set -ex \
    && chmod u+x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer \
    # 查看PHP版本和扩展
    && php -v \
    && php -m \
    && php --ri swoole \
    && php --ri redis \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

WORKDIR /opt/www

EXPOSE 9601
