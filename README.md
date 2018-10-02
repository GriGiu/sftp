# sftp server
With this docker, you can create a SSH and SFTP users. SFTP users are restricted to their "home" directories and defined as "non-interactive shell user" which means they cannot login to the server as a "normal" user. They are allowed to use sftp protocol only.
# user.sh
This file creates master user, assigns /home/master as home, ssh as user group and sets master as password. Apart from all that, it grants rm, mkdir, chown, useradd, deluser and chpasswd command usages with the help of sudo command. This user now can create a new SSH user and revert what he has done. The sftp group will be used for new SFTP users.

#Test

#Run container
$ docker-compose up -d
 
IMAGE               COMMAND                  PORTS                  NAMES
sftp_server         "/usr/local/bin/entr…"   0.0.0.0:2222->22/tcp   sftp_server_1

#SSH user login

Remember this user is responsible for creating new SSH and SFTP users.


$ ssh master@container-ip -p 22 # Password is master

#Add a new SFTP user

Login as "master" user and run commands below.


$ sudo mkdir /uploads/inanzzz
$ sudo mkdir /uploads/inanzzz/upload
$ sudo useradd -d /uploads/inanzzz -G sftp inanzzz -s /usr/sbin/nologin
$ echo "inanzzz:inanzzz" | sudo chpasswd
$ sudo chown inanzzz:sftp -R /uploads/inanzzz/upload

#SFTP user login

$ sftp inanzzz@container-ip
sftp>

#Client connection test

I am running the container in a Vagrant box and its IP address is 192.168.99.30. I had to add CONTAINER_IP localhost to /etc/hosts file in Vagrant box. Open up a FTP client (e.g. FileZilla) and use values below to connect.


- Host: 192.168.99.30
- Port: 2222
- Protocol: SFTP
- Logon Type: Ask for password
- Username: inanzzz
- Password: inanzzz
