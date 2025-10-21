# PHP 8.3
- https://www.php.net/
## Install
```bash
#!/bin/bash
echo "Add the following PHP PPA repository"
sudo add-apt-repository ppa:ondrej/php -y && sudo apt-get update

# PHPVERS=('8.1' '8.2' '8.3')
PHPVERS='8.3'
for PHPVER in "${PHPVERS[@]}"; do
	sudo apt-get install -y php$PHPVER libapache2-mod-php$PHPVER
	sudo apt-get install -y php$PHPVER-{fpm,curl,zip,intl,xmlrpc,soap,xml,gd,ldap,common,cli,mbstring,mysql,imagick,readline,tidy,redis,memcached,apcu,opcache,mongodb} 
done
```
## Config php.ini
```bash
#!/bin/bash
# PHPVERS=('8.1' '8.2' '8.3')
PHPVERS='8.3'
for PHPVER in "${PHPVERS[@]}"; do
	# Set PHP ini
	sed -i 's/memory_limit =.*/memory_limit = 512M/' /etc/php/$PHPVER/fpm/php.ini
	sed -i 's/post_max_size =.*/post_max_size = 512M/' /etc/php/$PHPVER/fpm/php.ini
	sed -i 's/upload_max_filesize =.*/upload_max_filesize = 512M/' /etc/php/$PHPVER/fpm/php.ini
	sed -i 's/;max_input_vars =.*/max_input_vars = 6000/' /etc/php/$PHPVER/fpm/php.ini

	sed -i 's/memory_limit =.*/memory_limit = 512M/' /etc/php/$PHPVER/apache2/php.ini
	sed -i 's/post_max_size =.*/post_max_size = 512M/' /etc/php/$PHPVER/apache2/php.ini
	sed -i 's/upload_max_filesize =.*/upload_max_filesize = 512M/' /etc/php/$PHPVER/apache2/php.ini
	sed -i 's/;max_input_vars =.*/max_input_vars = 6000/' /etc/php/$PHPVER/apache2/php.ini

	sed -i 's/memory_limit =.*/memory_limit = 512M/' /etc/php/$PHPVER/cli/php.ini
	sed -i 's/post_max_size =.*/post_max_size = 512M/' /etc/php/$PHPVER/cli/php.ini
	sed -i 's/upload_max_filesize =.*/upload_max_filesize = 512M/' /etc/php/$PHPVER/cli/php.ini
	sed -i 's/;max_input_vars =.*/max_input_vars = 6000/' /etc/php/$PHPVER/cli/php.ini
done
systemctl reload apache2

```
## Config apache .conf
```bash
cd /etc/apache2/sites-available
PHPVERS='8.3'
    <FilesMatch \.php$>
      # For Apache version 2.4.10 and above, use SetHandler to run PHP as a fastCGI process server
      SetHandler "proxy:unix:/run/php/php$PHPVER-fpm.sock|fcgi://localhost"
    </FilesMatch>
```


## Purge

```bash
sudo apt-get purge php8.*
sudo apt-get autoclean
sudo apt-get autoremove
```
