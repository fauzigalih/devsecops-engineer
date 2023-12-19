# Nginx Documentation

Note:
Use linux ubuntu

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
