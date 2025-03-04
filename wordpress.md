# installation LAMP wordpress et certbot

Commande pou installation du lamp

## LAMP

### Apache

Sous Debian
```
sudo apt-get install -y apache2
sudo systemctl enable apache2
sudo a2enmod rewrite
sudo a2enmod deflate
sudo a2enmod headers
sudo a2enmod ssl
sudo systemctl restart apache2
```

### PHP

```
sudo apt-get install -y apache2-utils
sudo apt-get install -y php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
```

Se rendre dans sudo nano /var/www/html/phpinfo.php et ajouter

```
<?php
phpinfo();
?>
```

### MariaDB

```
sudo apt-get install -y mariadb-server
sudo mariadb-secure-installation
```
réponse :
no
y
MOT DE PASSE
MOT DE PASSE
y
y
y
y

```
CREATE DATABASE wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO admin@localhost IDENTIFIED BY "MONTMOTDEPASSE";
FLUSH PRIVILEGES;
EXIT
```

```
<VirtualHost *:80>
    ServerName lamwansecu.alcyon.ovh
    DocumentRoot /var/www/html/wordpress

    <Directory /var/www/html/wordpress>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    # Logs
    ErrorLog ${APACHE_LOG_DIR}/wordpress-error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress-access.log combined
</VirtualHost>
```

### Wordpress

```
sudo curl -O https://wordpress.org/latest.tar.gz && \
sudo tar -xzf latest.tar.gz -C /var/www/html/ && \
rm latest.tar.gz
```
```
sudo chown -R www-data:www-data /var/www/html/wordpress && \
sudo chmod -R 755 /var/www/html/wordpress
```
admin
MONMOTDEPASSE
## SSL certbot

```
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d SOUSDOMAINE.alcyon.ovh
