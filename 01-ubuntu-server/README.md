# Ubuntu Server Documentation

Note:
symbol $ = access without user root
symbol # = access with user root

## Basic Security on Linux Server
Chage password root:
```
# passwd
```

Create new username:
```
# adduser newuser
```

Allow sudo on new username:
```
# usermod -aG sudo newuser
```

Change hostname:
```
# hostnamectl set-hostname new-servername
# hostnamectl
```

Set file hosts:
```
# nano /etc/hosts


127.0.0.1    localhost localhost.localdomain
127.0.1.1    new-servername
```

Create ssh authorized_keys:
```
# su newuser
$ ssh-keygen
$ cd ~/.ssh
$ cat id_rsa.pub >> authorized_keys
$ cat id_rsa	(copy key to local pc)
$ rm id_rsa*
```	

Block access ssh root and ssh auth-key login:
```
$ sudo nano /etc/ssh/sshd_config

PermitRootLogin no
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no

$ sudo systemctl restart ssh
```

Login ssh with auth-key:
```
$ chmod 600 auth-key-file
$ ssh -i auth-key-file admins@ip-server
```