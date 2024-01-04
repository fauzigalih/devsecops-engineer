# Gitlab Private Documentation
List:
* [Install Gitlab CE with Docker](#install-gitlab-comunity-edition-with-docker)

## Install Gitlab Comunity Edition with Docker
```
docker run --detach \
  --publish 1443:443 --publish 8081:80 --publish 1001:22 \
  --name gitlab \
  --restart always \
  --volume gitlab-config:/etc/gitlab \
  --volume gitlab-logs:/var/log/gitlab \
  --volume gitlab-data:/var/opt/gitlab \
  --shm-size 2gb \
  gitlab/gitlab-ce:latest
```
