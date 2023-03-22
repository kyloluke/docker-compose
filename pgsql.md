Success. You can now start the database server using:

    /usr/lib/postgresql/10/bin/pg_ctl -D /var/lib/postgresql/10/main -l logfile start

Ver Cluster Port Status Owner    Data directory              Log file
10  main    5432 down   postgres /var/lib/postgresql/10/main /var/log/postgresql/postgresql-10-main.log

# postgresql数据的导入和导出

 ### 从服务器下载
 - 下载
    * `scp kamome@39.104.189.215:/home/kamome/data/stag_kamome_bk20200821.sql.tar.gz .`
    * `scp centos@10.13.208.139:/data/apps/kamome/storage/logs/debug-2020-10-13.log 1013.log`
 - 解压
    * `tar zxf stag_kamome_bk20200821.sql.tar.gz`

### 导入数据
- `psql -d kamome -h 172.100.0.11 -p 5432 -U postgres -f kamome20200506.sql`

###### 扩展
```
sudo su - postgres  // 切换postgres账户， 如果是docker容器则无需添加sudo
psql                // 进入postgresql
\l                  // 查看所有的库
\c kamome;          // 进入kamome库
```

# postgresql 安装及配置

    安装 php 扩展 php7.2-pgsql

    sudo su - postgres                                                              切换到该用户  

    $ psql                                                                          直接进入数据库 

    $ psql -h 127.0.0.1 -p 5432 -U postgres -d kamome                               用 postgres 用户，登录本地数据库 kamome 库

    $ pg_restore -h 127.0.0.1 -p 5432 -U postgres -d kamome < 数据库文件            导入数据到 kamome 库

    $ psql -h 127.0.0.1 -p 5432 -U postgres -d kamome                               用 postgres 用户，登录本地数据库 kamome 库

    postgres=# \l                                                                   显示所有的库
    postgres=# \c kamome                                                            选择 kamome 库
    postgres=# \password postgres                                                   给 postgres 用户设置密码
    postgres=# drop database kamome;                                                删除库
    postgres=# create database kamome;                                              创建库
    postgres-# drop role kamome;                                                    删除角色
    postgres-# create role asa02appuser;                                            创建角色
    postgres-# ALTER USER postgres WITH PASSWORD 'postgres';                        修改用户密码

    进入到 刚才创建的 kamome 的库中
    postgres-# select * from pg_tables where schemaname = 'asa02appuser';           搜索 role='asa02appuser' 的表？
    postgres-# select * from asa02appuser.access_ip_info where deleted is null;     搜索 role的access_ip_info中 deleted 为null的
    postgres-# update asa02appuser.access_ip_info set ip='127.0.0.1' where id=3;    允许本地ip链接这个role的这个表


    再 docker 中配置的时候 使用网络配置的 config中的 gateway 172.100.0.1 apache的容器才能链接到 postgresql的容器

# 远程工具链接 pgsql
    在 pgdata（也就是在安装pg时指定的存放数据的文件见中）文件夹中，找到pg_hba.conf文件，并写入下面的内容：
    $ host    all        all         0.0.0.0/0       trust
    
    在 pgdata 文件夹中，找到postgresql.conf为文件，并修改下面的内容：
    $ listen_addresses = '*'     #允许任何的ip地址监听，并保存


    


    
    