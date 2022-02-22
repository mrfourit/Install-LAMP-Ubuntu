1. Install Mysql

	sudo apt-get install mysql-server

2. Install Apache

	sudo apt-get install apache2

3. Install PHP

	sudo apt-get install software-properties-common
	sudo add-apt-repository ppa:ondrej/php
	sudo apt update
	sudo apt install php7.1 libapache2-mod-php7.1 php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-soap php7.1-gd php7.1-xml php7.1-intl php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-zip php7.1-curl

4. Update password root mysql
	sudo mysql
	SELECT user,authentication_string,plugin,host FROM mysql.user;
	ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
	FLUSH PRIVILEGES;

5. Install phpmyadmin
	cd /usr/share
	wget https://files.phpmyadmin.net/phpMyAdmin/5.1.1/phpMyAdmin-5.1.1-all-languages.zip
	unzip phpMyAdmin-5.1.1-all-languages.zip
	mv phpMyAdmin-5.1.1-all-languages phpmyadmin
	sudo chmod -R 755 phpmyadmin
	sudo nano /etc/apache2/sites-available/000-default.conf
	```markdown
	Alias /phpmyadmin "/usr/share/phpmyadmin/"
<Directory "/usr/share/phpmyadmin/">
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>
```
	mv config.sample.inc.php config.inc.php
    nano config.inc.php
    $cfg['blowfish_secret'] = 'L.cWeE{beVu9}yHQuHz3ki5ysndddddl';
    $cfg['TempDir'] = "/usr/share/phpmyadmin/tmp/";
    mkdir tmp
    chmod 777 tmp
    sudo service apache2 restart

6. Enable mode apache

	sudo a2enmod rewrite

	sudo a2enmod ssl

7. Install ssh server

	sudo apt-get install openssh-server

8. Config vhost
```markdown
 <Directory /var/www/html>
      Options Indexes FollowSymLinks MultiViews
      AllowOverride All
      Order allow,deny
      allow from all
      Require all granted
    </Directory>
```

9. Install letsencrypt
	sudo add-apt-repository ppa:certbot/certbot
	sudo apt install python-certbot-apache
	sudo certbot --apache -d your_domain -d www.your_domain

	0 0 1 * * certbot renew && service apache2 restart
