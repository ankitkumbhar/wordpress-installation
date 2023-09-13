# wordpress-installation
### Install PHP
```
sudo apt update
sudo apt install php \
    php-bcmath \
    pc-curl \
    php-imagick \
    php-intl \
    php-json \
    php-mbstring \
    php-mysql \
    php-xml \
    php-zip
```

### Install apache
```
sudo apt install apache2 \
			 ghostscript \
			 libapache2-mod-php
```

### Create installation directory
```
sudo mkdir -p /var/www/html
sudo chowm www-data: /var/www/html
```

### Download wordpress file from wordpress.org:
```
wget http://wordpress.org/latest.tar.gz
```

### Extract wordpress zip:
```
tar -xvzf latest.tar.gz
```

### Move extracted wordpress directory to workspace
```
sudo mv wordpress /var/www/html
```

### Set appropriate permission
```
sudo chown -R $USER:$USER /var/www/html/wordpress
```

### Configure wordpress:
1. Set virtual host for wordpress:
```
sudo nano /etc/apache2/site-available/wordpress.conf
```

2. copy below code to wordpress.conf file
```
<VirtualHost *:80>
    ServerName <app.example.com>
    DocumentRoot /var/www/html/wordpress
    <Directory /var/www/html/wordpress>
        Options FollowSymLinks
        AllowOverride All
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /var/www/html/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
    #ErrorLog ${APACHE_LOG_DIR}/error.log
    #CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

3. Enable site with command:
```
sudo a2ensite wordpress
```

4. Enable URL rewriting with command
```
sudo a2enmod rewrite
```

5. Disable the default site with command
```
sudo a2dissite 000-default
```

### Add DNS and ip in /etc/hosts
- sudo nano /etc/hosts
- add below line to the bottom of the hosts:
```
127.0.0.1   app.example.com
```

### Reload apache server
```
sudo service apache2 reload
```

### Configure database
```
sudo mysql -u root -p
```

```
mysql> CREATE DATABASE wordpress;
mysql> CREATE USER wordpressuser IDENTIFIED BY '<password>'
mysql> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER
    -> ON wordpress.*
    -> TO wordpressuser;
mysql> FLUSH PRIVILEGES;
mysql> exit;
```

### Configure wordpress to use mysql database
- Copy wp-config-sample.php to wp-config.php
    ```
    sudo cp /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
    ```
- Update DB_NAME, DB_USER, DB_PASSWORD, DB_HOST
- Now visit localhost/wp-login.php and update wordpress username and password