# Code Server Ubuntu

## Getting Started

Create a Ubuntu VPS using Digital Ocean, Linode, or Amazon Lightsail.

Setup SSH key into that server when creating.

Connect to that server using `root` access and change its password.

```sh
ssh root@255.255.255.255
passwd
```

When the connection hang up, force close it by pressing `Enter ~.`.

Create new user and add it to `sudo` group.

```sh
adduser yourusername
usermod -aG sudo yourusername
```

Copy your SSH key from your computer.

```sh
# copy using mac
pbcopy < ~/.ssh/id_rsa.pub

# copy using linux
xclip -sel clip < ~/.ssh/id_rsa.pub
```

Paste that into that user

```sh
mkdir /home/yourusername/.ssh`

vim /home/yourusername/.ssh/authorized_keys

# add your SSH key into it
```

Connect again using `yourusername` access.

```sh
ssh yourusername@255.255.255.255

# test the sudo access
sudo ls -la /root
```

Install MariaDB/MySQL server & mycli.

```sh
# update packages
sudo apt update

# install
sudo apt install mariadb-server mycli

# run secure setup
sudo mysql_secure_installation
```

Restart database server if necessary.

```sh
service mysql restart
```

Verify that MariaDB is operational.

```sh
# check the service status
systemctl status mariadb.service

# loging using root and secure setup password
sudo mysql -u root -p
```

---

## Use the Database

Create new database user, and grant all access.

### Login with root using `mysql`

```sh
sudo mysql -u root -p yourpassword
```

### Create new user

```sql
CREATE USER 'yourusername'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON *.* to 'haidar'@'localhost' WITH GRANT OPTION;
CREATE USER 'yourusername'@'%' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON *.* to 'haidar'@'%' WITH GRANT OPTION;
exit;
```

### Login with new user using `mycli`

```sh
mycli -u haidar -p yourpassword
```

### Create and use initial database

```sql
SHOW DATABASES;
CREATE DATABASE yourdatabase;
SHOW DATABASES;
USE yourdatabase;
```

---

## Use the database remotely

Edit MariaDB config.

```sh
sudo vim /etc/mysql/mariadb.cnf
```

Add its content with this `bind-address` configuration.

```txt
[mysqld]

bind-address=0.0.0.0
```

Restart database server.

```sh
sudo service mariadb restart

# open TCP connection on port 3306
sudo ufw allow 3306/tcp

# restart the firewall
sudo service ufw restart
```

Connect again remotely

```sh
# using mysql
mysql -u yourusername -h www.xxx.yyy.zzz -p

# using mycli
mycli -u yourusername -h www.xxx.yyy.zzz -p yourpassword
```
