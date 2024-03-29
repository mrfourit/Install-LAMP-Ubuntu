1. Install Mysql

	```markdown
	sudo apt-get install mysql-server
	```

2. Install Apache

	```markdown
	sudo apt-get install apache2
	```

3. Install PHP

	```markdown
	sudo apt-get install software-properties-common
	```

	```markdown
	sudo add-apt-repository ppa:ondrej/php
	```

	```markdown
	sudo apt update
	```

	```markdown
	sudo apt install php7.1 libapache2-mod-php7.1 php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-soap php7.1-gd php7.1-xml php7.1-intl php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-zip php7.1-curl
	```
	
	```markdown
	sudo apt install php7.4 libapache2-mod-php7.4 php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-gd php7.4-xml php7.4-intl php7.4-mysql php7.4-cli php7.4-mcrypt php7.4-zip php7.4-curl
	```

4. Update password root mysql
	```markdown
	sudo mysql
	```

	```markdown
	SELECT user,authentication_string,plugin,host FROM mysql.user;
	```

	```markdown
	ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
	```
	
	```markdown
	FLUSH PRIVILEGES;
	```

5. Install phpmyadmin
	```markdown
	cd /usr/share
	```
	
	```markdown
	wget https://files.phpmyadmin.net/phpMyAdmin/5.1.1/phpMyAdmin-5.1.1-all-languages.zip
	```
	
	```markdown
	unzip phpMyAdmin-5.1.1-all-languages.zip
	```
	
	```markdown
	mv phpMyAdmin-5.1.1-all-languages phpmyadmin
	```
	
	```markdown
	sudo chmod -R 755 phpmyadmin
	```
	
	```markdown
	sudo nano /etc/apache2/sites-available/000-default.conf
	```
	
	```markdown
	Alias /phpmyadmin "/usr/share/phpmyadmin/"
	<Directory "/usr/share/phpmyadmin/">
		Order allow,deny
		Allow from all
		Require all granted
	</Directory>
	```

	```markdown
	cd phpmyadmin
	```

	```markdown
	mv config.sample.inc.php config.inc.php
	```
	
	```markdown
	nano config.inc.php
	```
	
	```markdown
	$cfg['blowfish_secret'] = 'L.cWeE{beVu9}yHQuHz3ki5ysndddddl';
	```
	
	```markdown
	$cfg['TempDir'] = "/usr/share/phpmyadmin/tmp/";
	```
	
	```markdown
	mkdir tmp
	```
	
	```markdown
	chmod 777 tmp
	```
	
	```markdown
	sudo service apache2 restart
	```

6. Enable mode apache
	```markdown
	sudo a2enmod rewrite
	```
	
	```markdown
	sudo a2enmod ssl
	```

7. Install ssh server
	```markdown
	sudo apt-get install openssh-server
	```

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
	```markdown
	sudo add-apt-repository ppa:certbot/certbot
	```
	
	```markdown
	sudo apt install python-certbot-apache
	```
	
	```markdown
	sudo certbot --apache -d your_domain -d www.your_domain
	```

	```markdown
	0 0 1 * * certbot renew && service apache2 restart
	```
10. Set PHP version
	```markdown
	sudo update-alternatives --config php
	```
	```markdown
	sudo a2dismod php7.1
	```
	```markdown
	sudo a2enmod php7.4
	```

11. Open port oracle
	```markdown
	sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
	```
	
	```markdown
	sudo netfilter-persistent save
	```
12. Mysql command
	```markdown
	CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
	```
	```markdown
	GRANT ALL PRIVILEGES ON db.* TO 'root'@'%'  WITH GRANT OPTION;
	```
	```markdown
	FLUSH PRIVILEGES;
	```
14. Install FTP
	```markdown
	sudo apt-get install proftpd
	```
15. Config FTP
 	```markdown
	useradd <username> -d /var/www/ -s /bin/false
	```
	```markdown
	passwd <username>
	```
	```markdown
	echo '/bin/false' | sudo tee -a /etc/shells > /dev/null
	```
	```markdown
	sudo nano /etc/proftpd/proftpd.conf
	```
	```markdown
	<Limit LOGIN>
		AllowUser elinkgate
		DenyALL
	</Limit>
	DefaultRoot <Source> elinkgate
	<Directory source>
		Umask 022 022
		AllowOverwrite on
		<Limit MKD STOR  XMKD RNRF RNTO RMD XRMD CWD>
			DenyAll
		</Limit>
		<Limit STOR CWD MKD>
			AllowAll
		</Limit>
	</Directory>
	```
	```markdown
	sudo /etc/init.d/proftpd start
	```
	```markdown
	sudo /etc/init.d/proftpd stop
	```
	```markdown
	sudo /etc/init.d/proftpd restart
	```
16. Install Swap
	```markdown
	swapon --show
	```
	```markdown
	sudo fallocate -l 2G /swapfile
	```
	```markdown
	ls -lh /swapfile
	```
	```markdown
	sudo chmod 600 /swapfile
	```
	```markdown
	sudo mkswap /swapfile
	```
	```markdown
	sudo swapon /swapfile
	```
	```markdown
	sudo swapon --show
	```
	```markdown
	echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
	```
	```markdown
	sudo sysctl vm.swappiness=10
	```
	```markdown
	sudo sysctl vm.vfs_cache_pressure=50
	```
	```markdown
	echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
	```
	```markdown
	echo 'vm.vfs_cache_pressure=50' | sudo tee -a /etc/sysctl.conf
	```
17. Install Ioncube
	```markdown
	cd /tmp
	```
	-------------------- For 64-bit System --------------------
	```markdown
	wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
	```
	-------------------- For 32-bit System --------------------
	```markdown
	wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86.tar.gz
	```
	```markdown
	tar -zxvf ioncube_loaders_lin_x86*
	```
	```markdown
	cd ioncube/
	```
	```markdown
	php -i | grep extension_dir
	```
	```markdown
	sudo cp /tmp/ioncube/ioncube_loader_lin_7.0.so [extension_dir]
	```
	```markdown
	sudo nano /etc/php/[php_version]/cli/php.ini
	```
	```markdown
	zend_extension = [extension_dir]/ioncube_loader_lin_7.0.so
	```
	```markdown
	sudo service apache2 restart
	```
18. Install Imagick
	```markdown
	sudo apt install imagemagick
	```
	```markdown
	sudo apt install php7.4-imagick
	```
