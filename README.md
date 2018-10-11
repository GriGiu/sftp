# sftp server


With this docker, you can create a SSH and SFTP users. SFTP users are restricted to their "home" directories and defined as "non-interactive shell user" which means they cannot login to the server as a "normal" user. They are allowed to use sftp protocol only.
# user.sh
This file creates master user, assigns /home/master as home, ssh as user group and sets master as password. Apart from all that, it grants rm, mkdir, chown, useradd, deluser and chpasswd command usages with the help of sudo command. This user now can create a new SSH user and revert what he has done. The sftp group will be used for new SFTP users.

# Test

## Run container
$ docker-compose up -d
 
IMAGE               COMMAND                  PORTS                  NAMES
sftp_server         "/usr/local/bin/entr…"   0.0.0.0:2222->22/tcp   sftp_server_1

## SSH user login

Remember this user is responsible for creating new SSH and SFTP users.


$ ssh master@container-ip -p 2222 # Password is master

## Add a new SFTP user

Login as "master" user and run commands below.


$ sudo mkdir /uploads/grigiu

$ sudo mkdir /uploads/grigiu/upload

$ sudo useradd -d /uploads/grigiu -G sftp grigiu -s /usr/sbin/nologin

$ echo "grigiu:grigiu" | sudo chpasswd

$ sudo chown grigiu:sftp -R /uploads/grigiu/upload


## SFTP user login

$ sftp -P 2222 grigiu@container-ip
sftp>

## Client connection test

I am running the container in a VM and its IP address is 192.168.99.30. 
Open up a FTP client (e.g. FileZilla) and use values below to connect.


- Host: 192.168.99.30
- Port: 2222
- Protocol: SFTP
- Logon Type: Ask for password
- Username: grigiu
- Password: grigiu
