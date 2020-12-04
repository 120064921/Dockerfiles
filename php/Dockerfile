FROM php:7.2.4-cli-alpine3.7

# 设置时区
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

# 修改apk源
RUN echo http://mirrors.ustc.edu.cn/alpine/v3.7/main > /etc/apk/repositories && echo http://mirrors.ustc.edu.cn/alpine/v3.7/community >> /etc/apk/repositories

# 安装系统依赖库
RUN apk update && apk upgrade
RUN apk add m4 autoconf make gcc g++ linux-headers

# 安装php扩展
ADD ./php-exts/swoole-4.5.5.tgz /tmp/
RUN cd /tmp/swoole-4.5.5 && phpize && ./configure && make && make install
RUN docker-php-ext-enable swoole

ADD ./php-exts/redis-4.2.0.tgz /tmp/
RUN cd /tmp/redis-4.2.0 && phpize && ./configure && make && make install
RUN docker-php-ext-enable redis

RUN docker-php-ext-install pdo_mysql \
	&& docker-php-ext-install bcmath \
	&& docker-php-ext-install sockets \
	&& docker-php-ext-install zip \
	&& docker-php-ext-install sysvmsg

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