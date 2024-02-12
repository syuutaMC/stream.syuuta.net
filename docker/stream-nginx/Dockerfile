FROM almalinux/9-base
WORKDIR /app

# init
RUN dnf update -y \
  && dnf install -y zip unzip gcc pcre-devel openssl openssl-devel glibc-langpack-ja.x86_64 \
  && localectl set-locale LANG=ja_JP.utf8 \
  && timedatectl set-timezone Asia/Tokyo

# install Nginx
RUN wget https://github.com/arut/nginx-rtmp-module/archive/master.zip \
  && unzip master.zip \
  && wget http://nginx.org/download/nginx-1.25.3.tar.gz \
  && tar zxf nginx-1.25.3.tar.gz

WORKDIR /app/nginx-1.25.3

RUN ./configure --sbin-path=/usr/sbin/nginx \
  --conf-path=/etc/nginx/nginx.conf \
  --error-log-path=/var/log/nginx/error.log \
  --http-log-path=/var/log/nginx/access.log \
  --pid-path=/var/run/nginx.pid \
  --lock-path=/var/run/nginx.lock \
  --prefix=/usr/local/nginx \
  --add-module=/app/nginx-rtmp-module-master \
  && make \
  && make install

ENTRYPOINT [ "nginx" ]