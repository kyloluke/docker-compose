FROM newdeveloper/apache-php-composer

# just for china
RUN sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list

# Install dependencies
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt install tzdata
ENV TZ "Asia/Tokyo"
RUN echo "Asia/Tokyo" > /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata 
RUN apt-get install -y git curl apache2 php libapache2-mod-php php-mysql php-mbstring php-xml php-gd php-zip php-curl php-tidy
RUN apt-get install -y vim nano gnupg
RUN apt install -y php-gmp
RUN a2enmod rewrite
RUN chown -R www-data:www-data /var/www/html/
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ARG DEBIAN_FRONTEND=noninteractive
EXPOSE 80 443
# カスタマイズ　コンフィグファイル　を利用する
COPY ./conf.d/apache2.conf /etc/apache2/apache2.conf
COPY ./conf.d/wacor.conf /etc/apache2/sites-enabled/wacor.conf
