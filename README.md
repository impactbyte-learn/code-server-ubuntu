# Code Server Ubuntu

## Getting Started

Create a Ubuntu VPS using DigitalOcean, Linode, or Amazon Lightsail.

Setup SSH key into that server when creating.

Connect to that server using `root` access.

```sh
ssh root@255.255.255.255
passwd
```

Create new user.

```sh
adduser yourusername
usermod -aG sudo yourusername
```

Copy your SSH key into that user.

```sh
# pbcopy < ~/.ssh/id_rsa.pub
mkdir /home/yourusername/.ssh`
vim /home/yourusername/.ssh/authorized_keys
```

Connect again using `yourusername` access.

```sh
ssh yourusername@255.255.255.255
sudo ls -la /root
```

Install MariaDB/MySQL server & mycli.

```sh
sudo apt update
sudo apt install mariadb-server mycli
sudo mysql_secure_installation
```

Restart database server if necessary.

```sh
service mysql restart
```

Verify that MariaDB is operational.

```sh
systemctl status mysql.service
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

Edit its content.

```txt
[mysqld]

bind-address=0.0.0.0
```

Restart database server.

```sh
sudo service mariadb restart
sudo ufw allow 3306/tcp
sudo service ufw restart
```

Connect again remotely

```sh
mysql -u yourusername -h www.xxx.yyy.zzz -p
mycli -u yourusername -h www.xxx.yyy.zzz -p yourpassword
```
