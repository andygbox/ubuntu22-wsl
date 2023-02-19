# LAMP stack on WSL2 (Ubuntu 22.04) - Apache, MySQL, PHP, PhpMyAdmin

## A - Install Apache

```sh
sudo apt-get update && sudo apt-get upgrade 
sudo apt-get install -y apache2 apache2-utils
```
### Start apache server and verify the version

```sh
sudo service apache2 start 
apachectl -v
```

### Goto browser and hit http://localhost/
![apache2 localhost](https://github.com/andygbox/ubuntu22-wsl/blob/48ea6817b079844c9b2c129ecbc534084f2a4fbc/lamp01.png?raw=true)

## B - Install MySQL Server 8

```sh
sudo apt-get install -y mysql-server php-mysql
sudo service mysql restart
```

### Provide MySQL root password

```sh
sudo mysql
  #> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'wsl2p@SS';
  #> quit
```

### Secure MySQL Installation

```sh
sudo mysql_secure_installation
    #> Enter password for user root: wsl2p@SS
    #> Change the password for root: N
    #> Validate password component: N
    #> Remove anonymous users: Y
    #> Disallow root login remotely: Y
    #> Remove test database and access to it: Y
    #> Reload privilege tables now: Y
sudo service mysql stop
sudo usermod -d /var/lib/mysql mysql
sudo service mysql start
```

### Allow remote root login
```sh
sudo mysql -u root -pwsl2p@SS -e "UPDATE mysql.user SET plugin = 'mysql_native_password' WHERE User = 'root'; FLUSH PRIVILEGES;"
```

## C - Install Php with Extensions
### Ubuntu 22.04 comes with the latest PHP 8.1 repository added

```sh
sudo apt install -y php8.1 libapache2-mod-php8.1 php8.1-common php8.1-mysql \
php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli \
php8.1-imap php8.1-mbstring php8.1-opcache php8.1-apcu php8.1-soap \
php8.1-zip php8.1-intl php8.1-bcmath php8.1-bz2 php8.1-gmp zip unzip wget
```

### Modify PHP Configurations for better performance

```sh
sudo nano /etc/php/8.1/apache2/php.ini

upload_max_filesize = 96M
post_max_size = 64M
memory_limit = 512M
max_input_vars = 3000
```

### Now, Restart the Apache Server

```sh
sudo service apache2 restart
```

## D - Verify the Php Extensions are Enabled

### Deploy the php info page on apache’s document root

```sh
cd /var/www/html

sudo nano info.php
   <?php phpinfo();
```

### Access the web page from the browser with the URL: http://localhost/info.php, and search the page for modules and extensions like opcache, apcu etc.
![php8.1 localhost](https://github.com/andygbox/ubuntu22-wsl/blob/bfa003af22a9514841278e1985bb5ac82289f5bc/lamp02.png?raw=true)

## E - Install phpMyadmin for Database access

```sh
sudo apt-get install -y phpmyadmin
    #> Use apache2
    #> Configure db with dbconfig-common: No
```

### Check the phpMyadmin configuration files. It should be softlink of /etc/phpmyadmin/apache.conf

```sh
ls -la /etc/apache2/conf-available/phpmyadmin.conf
```

![phpmyadmin symlink](https://github.com/andygbox/ubuntu22-wsl/blob/9ad5a1d9cae3043b67a4bbff1057b862e98a2e97/lamp06.png?raw=true)
