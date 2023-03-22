
# docker 阿里云镜像一键安装docker脚本
    
    sudo sh get-docker.sh --mirror Aliyun #以阿里云镜像安装get-docker.sh脚本内容

    sudo apt install -y openssh-server

    共享硬盘时 使用 sudo usermod -aG docker your-user

# image 和 container 的关系

    image： 程序的一个定义或者一个定义             class

    container： 基于 image 生成的一个程序的实例    object

    一个 image 可以生成多个 container


# docker 镜像源设置

    编辑  vim /etc/docker/daemon.json 写入以下内容：
    
    {
        "registry-mirrors": [
                "https://1nj0zren.mirror.aliyuncs.com",
                "https://docker.mirrors.ustc.edu.cn",
                "http://f1361db2.m.daocloud.io",
                "https://registry.docker-cn.com"
        ],
        "dns": ["119.29.29.29"]
    }


# docker 常用命令

    docker images                           存在的所有的镜像

    docker ps                               所有正在运行的容器

    docker ps -a                            所有的容器

    docker run imageName                    将镜像生成容器 每执行一次就会生成一个容器

    docker run -it imageName bash           使用镜像生成一个容器 并 进入容器的命令行

    docker run -d nginx                     启动并后台运行nginx容器

    docker run -d --name=nginx01 nginx      同个镜像启动多个容器时，需要指定容器名称

    docker run -p 80:80 nginx               端口映射，可以填写多个，前面是本机，后面是容器

    docker start containerId/containerName  启动一个已经存在的容器

    docker exec -it containerId bash        进入一个正在运行的容器的命令行,如果没在运行，则需要 docker start 

    docker exec -u 0 -it contanername bash  root身份进入容器

    docker stop containerId/containerName   停止一个正在运行中的容器

    docker rm containerId/containerName     删除一个已经存在的容器 不能删除正在运行中的容器
    
    docker rm containerId containerId       同时删除多个容器 

    docker rm -f containerId                强制删除一个运行中的容器

    docker rmi imageId/imageName            删除一个没有容器实例的镜像， 如果有容器，则先删除容器后才能删除镜像

    docker rm -f $(docker ps -aq)           删除所有的容器,无论运行与否

    docker cp /tmp/a.sh 容器ID:/root        复制文件从本地到容器

    docker attach containerId  关联一个正在运行的容器 如果是容器是nginx之类的，则会将后台模式切换为前台模式
# 实用命令
    cat /etc/os-release                 查看容器的信息  linux 信息

    -i:      input 输入放到容器里面，容器识别
    -t:      tty   分配命令行窗口 进入互动模式
    bash          进入到命令行       

# nginx容器中的配置文件位置
    /etc/nginx/conf.d/default.conf

# 容器中换源  其实就是 linux 换源
    /etc/apt/sources.list
    sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list


# 共享目录
    windows:    docker run -d -p 80:80 -v C:Users\lu\Desktop\docker_share:/usr/share/nginx/html nginx
    mac/linux:  docker run -d -p 80:80 -v $(pwd):/usr/share/nginx/html   当前目录映射到指定目录 nginx

# 创建自己的镜像
    docker build -t ubuntu-cn .   // -t tag的意思  .是当前目录的目录查找 Dockerfile 并执行

# 其他
    echo $PHP_INI_DIR 检测php配置文件目录

    php --ini  查看配置文件位置信息

    php -ini | grep upload  查看配置文件的指定项的参数


# 网络链接
    docker network ls                               网络列表

    docker network create 链接名称                  创建网络

    docker network connect 网络名称 容器名称         将一个容器加入一个网络  需要指定ip地址，否则可能添加不成功

    docker network inspect 网络名称                 审查网络内容  该网络中有哪些容器

    在同一个网络中的两个容器，可以使用容器名称 ping 通，也就是说容器的名字会被解析成ip地址


