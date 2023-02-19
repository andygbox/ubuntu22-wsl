# LAMP stack on WSL2 (Ubuntu 22.04) - Apache, MySQL, PHP, PhpMyAdmin

## Install Apache

```sh
sudo apt-get update && sudo apt-get upgrade 
sudo apt-get install -y apache2 apache2-utils
```
### Start apache server and verify the version

```sh
sudo service apache2 start 
apachectl -v
```

### Go to Bowser and hit http://localhost/
