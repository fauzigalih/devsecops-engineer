# Nginx Documentation

Note:
Use linux ubuntu<br>
$ = user without root<br>
\# = user with root<br>

List:
* [Install Nginx Basic](#install-nginx-basic)
* [Install Nginx via Docker](#install-nginx-via-docker)
* [Nginx Proxy Configuration](#nginx-proxy-configuration)
* [Nginx Php Fpm Configuration](#nginx-php-fpm-configuration)
* [Redirect Http to Https](#redirect-http-to-https)
* [Install ModSecurity on Nginx (Nginx WAF)](#install-modsecurity-on-nginx-nginx-waf)

## Install Nginx Basic
update package ubuntu:
```
$ sudo apt update
```

install nginx:
```
$ sudo apt install nginx
```

check status nginx:
```
$ sudo systemctl status nginx
```

config file in path `/etc/nginx/sites-enabled/`


## Install Nginx via Docker
pull docker image nginx:
```
$ docker pull nginx:latest
```

running container nginx:
```
$ docker run -d --name nginx -p 80:80 -p 443:443 nginx:latest
```

check status nginx:
```
$ docker ps | grep nginx
```

config file on container, in path `/etc/nginx/conf.d/`:
```
$ docker exec -it nginx /bin/sh
```

## Nginx Proxy Configuration
create config file:
```
# cd /etc/nginx/sites-available/proxy
```

example configuration for port proxy:
```
server {
        listen 80;
        listen [::]:80;

        server_name sub.domain.com;

        location / {
                proxy_pass http://localhost:3000;
        }
}
```

create symlink:
```
# ln -s /etc/nginx/sites-available/proxy /etc/nginx/sites-enabled/
```

restart service nginx:
```
# systemctl restart nginx
```

## Nginx Php Fpm Configuration
create config file:
```
# cd /etc/nginx/sites-available/app
```

example configuration for application use php-fpm:
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/app;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name sub.domain.com;

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        }
}
```

create symlink:
```
# ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled/
```

restart service nginx:
```
# systemctl restart nginx
```

## Redirect Http to Https
create config file:
```
# cd /etc/nginx/sites-available/redirect
```

example configuration for application use php-fpm:
```
server {
        listen 80;
        listen [::]:80;
        server_name sub.domain.com;
        return 301 https://sub.domain.com$request_uri;
}
server {
        listen 443 ssl;
        listen [::]:443 ssl;

        root /var/www/app;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name sub.domain.com;

        ssl_certificate        /etc/nginx/ssl/ssl.crt;
        ssl_certificate_key    /etc/nginx/ssl/ssl.key;

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        }
}
```

create symlink:
```
# ln -s /etc/nginx/sites-available/redirect /etc/nginx/sites-enabled/
```

restart service nginx:
```
# systemctl restart nginx
```

## Install ModSecurity on Nginx (Nginx WAF)
update and upgrade package linux:
```
# apt update && apt upgrade
```

install nginx:
```
# apt install nginx
```

check version nginx:
```
# nginx -v
```

install dependencies required:
```
# apt-get install bison build-essential ca-certificates curl dh-autoreconf doxygen flex gawk git iputils-ping libcurl4-gnutls-dev libexpat1-dev libgeoip-dev liblmdb-dev libpcre3-dev libpcre++-dev libssl-dev libtool libxml2 libxml2-dev libyajl-dev locales lua5.3-dev pkg-config wget zlib1g-dev zlibc libxslt1-dev libgd-dev
```

clone ModSecurity on directory `/opt`:
```
# cd /opt && git clone https://github.com/SpiderLabs/ModSecurity
```

run init and update submodule on directory `/opt/ModSecurity`:
```
# cd ModSecurity
```
```
# git submodule init
```
```
# git submodule update
```

run script `build.sh` on directory `/opt/ModSecurity`:
```
# ./build.sh
```

run `configure` file: on directory `/opt/ModSecurity`
```
# ./configure
```

run `make` command to build ModSecurity:
```
# make
```

after build ModSecurity, now install ModSecurity:
```
# make install
```

download ModSecurity Nginx Connector:
```
# cd /opt && git clone https://github.com/SpiderLabs/ModSecurity-nginx.git
```

building ModSecurity for Nginx, you must verify version nginx. example `nginx version: nginx/1.18.0`:
```
# nginx -v
```
```
# cd /opt && wget http://nginx.org/download/nginx-1.18.0.tar.gz && tar -xzvf nginx-1.18.0.tar.gz && cd /opt/nginx-1.18.0
```

compile security module with `configure argument` on command `nginx -V`:
```
# nginx -V
```
example command: `./configure --add-dynamic-module=../ModSecurity-nginx <Configure Argument Nginx>`:
```
# ./configure --add-dynamic-module=../ModSecurity-nginx --with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-lUTckl/nginx-1.18.0=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-compat --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_xslt_module=dynamic --with-stream=dynamic --with-stream_ssl_module --with-mail=dynamic --with-mail_ssl_module 
```
```
# make modules
```

create directory modules and copy ModSecurity:
```
# mkdir /etc/nginx/modules && cp objs/ngx_http_modsecurity_module.so /etc/nginx/modules
```

load module ModSecurity on `/etc/nginx/nginx.conf`:
```
load_module /etc/nginx/modules/ngx_http_modsecurity_module.so;
```
example file:
```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;
load_module /etc/nginx/modules/ngx_http_modsecurity_module.so;

events {
        worker_connections 768;
        # multi_accept on;
}
```

clone OWASP-CRS on `/opt`:
```
# cd /opt && git clone https://github.com/coreruleset/coreruleset modsecurity-crs
```

rename file ModSecurity-CRS:
```
# cd /opt/modsecurity-crs && mv crs-setup.conf.example crs-setup.conf && mv rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
```

move directory `modsecurity-crs` to `/usr/local/`:
```
# mv /opt/modsecurity-crs /usr/local/
```

create directory `modsec` and copy unicode mapping file:
```
# mkdir /etc/nginx/modsec && cp /opt/ModSecurity/unicode.mapping /etc/nginx/modsec
```

rename and copy `modsecurity.conf` to dir `/etc/nginx/modsec`:
```
# cd /opt/ModSecurity && mv modsecurity.conf-recommended modsecurity.conf && cp modsecurity.conf /etc/nginx/modsec/
```

change file `/etc/modsecurity/modsecurity.conf` on `SecRuleEngine DetectionOnly` to `SecRuleEngine On`:
```
SecRuleEngine On
```

create file `main.conf`:
```
# nano /etc/nginx/modsec/main.conf
```
and add include file on file `main.conf`:
```
Include /etc/nginx/modsec/modsecurity.conf
Include /usr/local/modsecurity-crs/crs-setup.conf
Include /usr/local/modsecurity-crs/rules/*.conf
```

test ModSecurity:
```
# nano /etc/nginx/sites-enabled/default
```
example configuration with ModSecurity:
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        modsecurity on;
        modsecurity_rules_file /etc/nginx/modsec/main.conf;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

restart nginx:
```
# systemctl restart nginx
```

check ModSecurity, if show error `403 Forbidden`, ModSecurity is successfully:
```
http://<IP ADDRESS>/?exec=/bin/bash
```

