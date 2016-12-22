FROM ubuntu:16.04

ADD nginx_1.11.7-1~xenial_amd64.deb /root/nginx_1.11.7-1~xenial_amd64.deb

RUN apt-get update && apt-get install -y --no-install-recommends wget && \
    wget -O - http://nginx.org/keys/nginx_signing.key | apt-key add - && \
    echo "deb http://nginx.org/packages/mainline/ubuntu/ xenial nginx" >> /etc/apt/sources.list.d/nginx.list && \
    echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu xenial main" >> /etc/apt/sources.list.d/php.list && \
    apt-key adv --recv-keys --keyserver keyserver.ubuntu.com E5267A6C && \
    apt update && apt -y upgrade && \
    apt install -y --no-install-recommends logrotate geoip-database libgeoip-dev libgeoip1 language-pack-en-base supervisor ca-certificates bzip2 && \
    apt install -y --no-install-recommends php7.0-fpm php7.0-gd php7.0-mysql php7.0-curl php7.0-xml php7.0-zip php7.0-intl php7.0-mcrypt php7.0-mbstring php-apcu php-imagick php7.0-json php7.0-bz2 php7.0-zip && \
    (dpkg -i /root/nginx_1.11.7-1~xenial_amd64.deb || true) && \
    apt-get -f -y install && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz -O /usr/share/GeoIP/GeoIP.dat.gz && \
    rm -f /usr/share/GeoIP/GeoIP.dat && \
    cd /usr/share/GeoIP && gunzip GeoIP.dat.gz && cd /

RUN mkdir -p /var/www/nextcloud && mkdir -p /var/nc_data && mkdir -p /var/www/letsencrypt && mkdir -p /usr/local/tmp/cache && mkdir /upload_tmp && mkdir /run/php && \
    chown -R www-data:www-data /upload_tmp

RUN rm -f /etc/nginx/conf.d/default.conf

ADD nginx.conf /etc/nginx/nginx.conf
ADD nextcloud.conf /etc/nginx/conf.d/nextcloud.conf
ADD php.ini /etc/php/7.0/fpm/php.ini
ADD www.conf /etc/php/7.0/fpm/pool.d/www.conf
ADD php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
ADD supervisord.conf /etc/supervisord.conf
ADD config.php /var/www/nextcloud/config/config.php

RUN wget https://download.nextcloud.com/server/releases/nextcloud-11.0.0.tar.bz2 -O /root/nextcloud-11.0.0.tar.bz2 && \
    tar -xjf /root/nextcloud-11.0.0.tar.bz2 -C /var/www && \
    rm -f /root/nextcloud-11.0.0.tar.bz2 && \
    chown -R www-data:www-data /var/www/nextcloud/config

VOLUME ["/var/nc_data", "/var/www/letsencrypt"]
EXPOSE 80 443
CMD ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]
