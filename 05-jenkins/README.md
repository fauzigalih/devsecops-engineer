# Jenkins Documentation
List:
* [Install Gitlab CE with Docker](#install-jenkins-master-with-docker)
* [Create Jenkins Agent with SSH](#create-jenkins-agent-with-ssh)

## Install Jenkins Master with Docker Compose
Create new directory:
```
cd ~
mkdir docker && cd docker
mkdir jenkins && cd jenkins
```

Create file `docker-compose.yaml` , and place script:
```
nano docker-compose.yaml
```
```
version: "3.8"

services:
  jenkins:
    container_name: jenkins
    image: jenkins/jenkins:alpine-jdk21
    ports:
      - "8080:8080"
    restart: on-failure
    volumes:
      - jenkins-data:/var/jenkins_home
    environment:
      - TZ=Asia/Jakarta

volumes:
  jenkins-data:
```

Running jenkins with docker compose:
```
docker compose up -d
```

Running jenkins in browser with http://<IP_SERVER>:<PORT_PUBLISH>, example `http://localhost:8080` <br>

Get code key setup jenkins on container jenkins:
```
docker exec -it jenkins /bin/bash
```

## Create Jenkins Agent with SSH
Install java version 21<br>

Create credential access on `Manage Jenkins -> Security -> Credentials -> Global -> Add Credentials`
```
if kind use `Username with password`:
username: <username_access>
password: <password_access>

if kind use `SSH Username with private key`, then generate ssh key on server location:
  ssh-keygen -t ed25519
  cd ~/.ssh
  cat id_ed25519.pub >> authorized_keys
username: <username_access>
privatekey: <id_ed25519>
passphrase: <passphrase>
```

Create new node in `Manage Jenkins -> Nodes -> New Node -> Permanent Agent` :
```
name: <node_name> - example: Agent1
number_of_executor: <number> - example: 4
remote_root_directory: <directory_path> - example: /home/users/docker/jenkins/jenkins-agent
usage: Use this node as much as possible
launch_method: Launch agent via SSH
host: <host_server> - example: 127.0.0.1
credentials: <credentials>
host_key_verification_strategy: Non verifying Verification Strategy
availability: Keep this agent online as much as possible
```
