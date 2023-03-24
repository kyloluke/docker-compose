FROM nginx:1.17

# RUN sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list && \
#     sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

# 更新软件源
RUN apt-get update && \
    apt-get install -y vim

# 自定义配置文件
COPY ./conf.d/kamome-cn.conf /etc/nginx/conf.d/kamome-cn.conf
COPY ./conf.d/kamome-86.conf /etc/nginx/conf.d/kamome-86.conf
COPY ./conf.d/kamome-asia.conf /etc/nginx/conf.d/kamome-asia.conf
COPY ./conf.d/wordpress.conf /etc/nginx/conf.d/wordpress.conf
COPY ./conf.d/swk.conf /etc/nginx/conf.d/swk.conf
COPY ./conf.d/laravel-test.conf /etc/nginx/conf.d/laravel-test.conf
COPY ./conf.d/laravel-shop.conf /etc/nginx/conf.d/laravel-shop.conf