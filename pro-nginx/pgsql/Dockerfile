FROM postgres:12

# RUN sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list && \
#     sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

# 更新软件源
RUN apt-get update && \
    apt-get install -y vim

    
# RUN mv /var/lib/postgresql/data/pg_hba.conf /var/lib/postgresql/data/pg_hba.conf.back
# COPY ./pgsql/pg_hba.conf /var/lib/postgresql/data/

COPY ./ext/kamome_bk20210315.sql.tar.gz /home
#COPY ./kamome20200327.sql /home

