
# 在 php:7.3-fpm 官方镜像的基础上进行定制  如果基于此创建镜像，同时会创建该官方镜像 也就是 php 他的 tag 是 
FROM php:7.3-fpm

# 给该镜像的系统换源 
# RUN sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list && \
#     sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

# 下载 composer 并且 更换镜像  需要较好的网络支持
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer
    # composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/  国内需要生效此项

RUN apt-get install && apt-get update && \
    apt-get install -y wget && \
    apt-get install -y vim

#RUN wget https://nodejs.org/dist/v14.15.0/node-v14.15.0.tar.gz && \
#    tar -xvf node-v14.15.0.tar.gz && \
#    apt-get install -y python && \
#    cd node-v14.15.0 && \
#    make && make install

# 下载npm
RUN apt-get install -y nodejs npm
# RUN npm config set registry https://registry.npm.taobao.org  国内需要生效此项

# php 扩展文件存放目录  /usr/local/lib/php/extensions/no-debug-non-zts-20180731/
# 安装 扩展
RUN apt-get install && apt-get update && \
    apt-get install -y git libzip-dev zip && \
    docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-install zip

# 安装 gd 扩展
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
# 安装 bcmath 扩展
RUN docker-php-ext-install -j$(nproc) bcmath

# 安装 pdo_mysql 扩展
RUN apt-get update && apt-get install -y libpq-dev && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install pdo_pgsql && \
    docker-php-ext-install pcntl && \
    docker-php-ext-install pgsql && \
    docker-php-ext-install sockets

# 安装 redis 扩展 http://pecl.php.net/package/redis
# laravel 的情况 config/database.php redis[client] 的值有一下两种
# 1. phpredis  需要phpredis扩展支持
# 2. predis  安装 predis/predis composer包 
# 我们预装 phpredis 扩展
WORKDIR /root
RUN wget https://pecl.php.net/get/redis-5.0.0.tgz && \
    tar zxvf redis-5.0.0.tgz && \
    cd redis-5.0.0 && \
    phpize && \
    ./configure --with-php-config=/usr/local/bin/php-config && \
    make && make install

# 加载自定义配置文件
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
COPY custom.ini $PHP_INI_DIR/conf.d/custom.ini


# 一些技巧
# docker-compose exec php npm install  在容器外，通过docker-compose 命令执行容器内的命令