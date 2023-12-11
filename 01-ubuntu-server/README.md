# Ubuntu Server Documentation

Note:<br>
symbol $ = access without user root<br>
symbol # = access with user root

## Installation Ubuntu Server with IP Static
Install ubuntu server with VirtualBox:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/7ed22c81-34a5-42d0-a6a2-08bde52b75dc)

Create new virtual machine:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/0c13b501-120b-42a3-8c9e-882d72d536bf)

Adjust memory and processor:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/efddc101-06cc-4084-87cb-a1cbe6840458)

Adjust storage:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/8d6c398b-df9f-47eb-aebf-f72d02d4a037)

Summary:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/b36f5420-3082-477d-a3cc-c5a1f40eb0a3)

Setting Network to Bridged Adaptor and select WiFi driver:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/ed2731f7-7fe2-4e12-a982-e7d0426bf29a)

Start VM, and select Install Ubuntu Server:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/4445a74b-6c5d-40dd-b3ed-f00423830f87)

Select language:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/35f9d0b3-b3ba-4789-9da2-09ff72b826c5)

Continue:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/7e8504bc-6141-4719-8e19-d8fa04372c31)

Keyboard COnfiguration:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/407b280a-acbc-4cc9-8a67-20e3d410cb5e)

Select ubuntu server:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/b53f1abe-8990-4b54-94b9-c7223e392632)

Config network connection:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/ff827411-638b-4ae8-be66-417f9115b525)

Continue:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/07ff137d-82a1-4648-9e86-f26f5ebde935)

Configure proxy:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/5cd5aa5b-b707-44cf-b30c-e82d75e71b90)

Continue:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/231663a1-987a-4b37-bb91-7f0edad3495b)

Storage configuration:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/2fe823d3-27b8-40e7-ab9a-78fa8e86dfec)

Continue:
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/7f6e0aee-ed19-491c-86dc-295942a5fc36)

Continue:
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/4199b31a-f2d5-4336-8c2c-8eb268b9a85d)

Profile setup:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/cad3dd96-09a7-44d9-9dba-c01030eebd1e)

Continue:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/6a89b427-e8d8-4d04-8efc-37bf2b7f7385)

SSH setup:<br>
![image](https://github.com/fauzigalih/devsecops-engineer/assets/64176403/e9b54fa4-4b53-4a14-8af5-97fd6b839613)

## Basic Security on Linux Server
Change password root:
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

Enable firewall & allow port ssh:
```
$ sudo ufw enable
$ sudo ufw allow 22/tcp
$ sudo ufw status
```
