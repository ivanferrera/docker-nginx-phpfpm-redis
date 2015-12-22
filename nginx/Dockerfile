FROM centos:centos7

ENV NGINX_VERSION 1.9.9

# install updates and prerequisites
RUN yum -y update && \
    yum install -y wget gcc gcc-c++ pcre-devel zlib-devel make unzip openssl-devel libaio-devel glibc glibc-devel glibc-headers libevent

# Create nginx user
RUN useradd --no-create-home -s /bin/false nginx

# download/compile/install nginx
RUN wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
    tar -xvzf nginx-${NGINX_VERSION}.tar.gz && \
    rm -f nginx-${NGINX_VERSION}.tar.gz && \
    cd nginx-${NGINX_VERSION}/ && \
    ./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/run/nginx.pid --lock-path=/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --user=nginx --group=nginx --without-http_autoindex_module --without-http_ssi_module --without-http_scgi_module --without-http_uwsgi_module --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_gzip_static_module --with-http_stub_status_module --with-file-aio --with-pcre --with-http_v2_module --with-cc-opt='-DTCP_FASTOPEN=23 -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -m64 -mtune=generic' && \
    make && \
    make install

# Create directory structure
RUN mkdir /var/cache/nginx && \
    mkdir /var/cache/nginx/client_temp && \
    mkdir /var/cache/pagespeed && \
    mkdir /etc/nginx/conf.d && \
    mkdir /etc/nginx/sites-available && \
    mkdir /etc/nginx/sites-enabled

# Copy default config files
COPY etc/ /etc/
RUN ln -sf /etc/nginx/sites-available/* /etc/nginx/sites-enabled/

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

VOLUME ["/var/cache/nginx"]

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
