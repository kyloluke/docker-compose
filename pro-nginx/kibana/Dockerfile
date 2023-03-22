FROM kibana:7.17.5

RUN sed -i 's/security.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list

RUN apt-get update && \
    apt-get install vim

# 拷贝配置文件
COPY ./kibana.yml //usr/share/kibana/config/kibana.yml