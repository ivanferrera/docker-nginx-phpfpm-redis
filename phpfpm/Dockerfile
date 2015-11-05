FROM centos:centos7

ENV PHP_VERSION 55
RUN rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm && \
    rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

RUN yum -y update && \
    yum -y install php${PHP_VERSION}w php${PHP_VERSION}w-cli php${PHP_VERSION}w-common php${PHP_VERSION}w-devel php${PHP_VERSION}w-fpm php${PHP_VERSION}w-gd php${PHP_VERSION}w-mbstring php${PHP_VERSION}w-mcrypt php${PHP_VERSION}w-mysqlnd php${PHP_VERSION}w-opcache php${PHP_VERSION}w-pdo php${PHP_VERSION}w-pear  php${PHP_VERSION}w-pecl-igbinary php${PHP_VERSION}w-pecl-igbinary-devel php${PHP_VERSION}w-pecl-redis php${PHP_VERSION}w-soap php${PHP_VERSION}w-xml php${PHP_VERSION}w-xmlrpc

# Modify config files
RUN sed -e 's/127.0.0.1:9000/9000/' \
        -e '/allowed_clients/d' \
        -e '/catch_workers_output/s/^;//' \
        -e '/error_log/d' \
        -i /etc/php-fpm.d/www.conf

RUN sed -i -e "s/;igbinary.compact_strings=On/igbinary.compact_strings=On/g" /etc/php.d/igbinary.ini && \
    sed -i -e "s/;session.serialize_handler=igbinary/session.serialize_handler=igbinary/g" /etc/php.d/igbinary.ini && \
    sed -i -e "s/php_value\[session.save_handler\] = files/php_value\[session.save_handler\] = redis/g" /etc/php-fpm.d/www.conf && \
    sed -i -e "s/\/var\/lib\/php\/session/tcp:\/\/redis:6379/g" /etc/php-fpm.d/www.conf

EXPOSE 9000

ENTRYPOINT /usr/sbin/php-fpm --nodaemonize