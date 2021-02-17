FROM debian:jessie

RUN echo 'Europe/Moscow' > /etc/timezone && \
    /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    groupadd -g 396 nginx && \
    useradd nginx -u 396 -g 396 -M -c 'Nginx web server' -s /sbin/nologin -d /var/lib/nginx && \
    mkdir -p /var/log/nginx /var/lib/nginx /var/cache/nginx /usr/share/nginx && \
    chown -R nginx:nginx /var/log/nginx /var/lib/nginx /var/cache/nginx && \
    ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log
ENV NGINX_VERSION 1.8.0
RUN apt-get update && \
    apt-get install -y wget unzip make gcc libzip-dev libssl-dev libpcre3-dev libldap2-dev
RUN mkdir /tmp/build && cd /tmp/build && \
    wget https://github.com/kvspb/nginx-auth-ldap/archive/master.zip && \
    unzip master.zip && \
    wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
    tar xf nginx-${NGINX_VERSION}.tar.gz && cd nginx-${NGINX_VERSION} && \
    ./configure \
    --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/run/nginx.pid \
    --lock-path=/var/run/nginx.lock \
    --http-client-body-temp-path=/var/cache/nginx/client_temp \
    --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
    --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
    --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
    --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
    --user=nginx \
    --group=nginx \
    --with-http_ssl_module \
    --with-http_realip_module \
    --with-http_addition_module \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_mp4_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_stub_status_module \
    --with-http_auth_request_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-file-aio \
    --with-http_spdy_module \
    --with-ipv6 \
    --add-module=../nginx-auth-ldap-master && \
    make && make install
RUN apt-get remove -y gcc wget make unzip && \
       rm -rf /var/lib/apt/lists/*
EXPOSE 80 443 8000 8080
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
