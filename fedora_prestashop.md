# Experimentation Prestashop Fedora : Essai 1

## Installation et activation du service mariadb

```
sudo dnf install mariadb mariadb-server
sudo systemctl enable --now mariadb.service
sudo mysql_secure_installation
```

## Création de la base de données mariadb Prestashop

```
mysql -u root -p
CREATE DATABASE prestashop;
GRANT ALL ON prestashop.* TO 'prestashop'@'localhost' IDENTIFIED BY '@is';
FLUSH PRIVILEGES;
EXIT;
```


## Installation du service httpd

```
dnf install httpd
systemctl enable --now httpd.service
firewall-cmd --add-service=http --add-service=https --permanent
firewall-cmd --reload
```

## Configuration de PHP

```
nano /etc/httpds/conf/httpd.conf
```
Dans le fichier :

```
<Directory />
AllowOverride All
Require all denied
<Directory>
```
Il faut également configurer le **ServerName** par l'IP du serveur.

```
dnf install php php-gd php-mbstring php-mcrypt php-mysqli php-curl php-xml php-cli
dnf install phpmyadmin
nano /etc/httpd/conf.d/phpMyAdmin.conf
```

Le sample du script de configuration de phpMyAdmin

```
<Directory /usr/share/phpMyAdmin/>
# AddDefaultCharset UTF-8

# <IfModule mod_authz_core.c>
# # Apache 2.4
# <RequireAny>
# Require ip 127.0.0.1
# Require ip ::1
# </RequireAny>
# </IfModule>
# <IfModule !mod_authz_core.c>
# # Apache 2.2
# Order Deny,Allow
# Deny from All
# Allow from 127.0.0.1
# Allow from ::1
# </IfModule>
Require all granted
</Directory>
```

---

```
dnf install php php-mysql
systemctl restart httpd.service
``` 


## Téléchargement de Prestashop

```
cd /tmp
wget https://www.prestashop.com/download/old/prestashop_1.6.1.5.zip
dnf install unzip
unzip prestashop_1.6.1.5.zip
mkdir /var/www/prestashop
chown -R apache: /tmp/prestashop
mv /tmp/prestashop/* /var/www/html
```
 
## Resultat

**FAIL**

---
# Experimentation Prestashop Fedora : Essai 2

## Configuration de SELinux

Passage de Enforce > Permissive
```
setenforce 0
```
Pour que le changement soit permanent, faire la modification dans le fichier ```/etc/selinux/conf```


## Installation du serveur AMP (apache,mariadb,php) Fedora 38

### Apache httpd

```
sudo dnf install httpd
sudo firewall-cmd --add-service=http --add-service=https --permanent
sudo firewall-cmd --reload
sudo systemctl enable --now httpd
``` 

###  Installation de PHP 8.0 et ses extensions

```
sudo dnf -y install http://rpms.remirepo.net/fedora/remi-release-38.rpm
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --set-enabled remi
sudo dnf module reset php -y
sudo dnf module -y install php:remi-8.0
sudo dnf -y install php-cli php-fpm php-mysqlnd php-zip php-devel php-gd php-mcrypt php-mbstring php-curl php-xml php-pear php-bcmath php-json php-intl
sudo systemctl enable --now php-fpm
sudo systemctl restart httpd
```

[Source](https://computingforgeeks.com/how-to-install-php-on-fedora/)

Modifier la ligne ```date.timezone = ``` du fichier __/etc/php.ini__ et y ajouter ```Europe/Paris``` 

[Source](https://www.php.net/manual/en/timezones.europe.php)


### Installation de Mariadb

```
sudo dnf install mariadb-server
sudo nano /etc/my.cnf.d/mariadb-server.cnf   # Ajouter la ligne character-set-server=utf8 dans la section [mysqld]
sudo systemctl enable --now mariadb
sudo mysql_secure_installation
mysql -u root -p
```

## Configuration et installation de Prestashop

### Configuration de la table prestashop

```
CREATE DATABASE prestashop;
GRANT ALL PRIVILEGES ON prestashop.* TO 'presta_user'@'local_user' IDENTIFIED BY "mot de passe sécurisé";
FLUSH PRIVILEGES;
EXIT;
```

### Téléchargement et configuration de Prestashop

```
cd /tmp
wget https://github.com/PrestaShop/PrestaShop/releases/download/8.1.2/prestashop_8.1.2.zip
unzip prestashop_8.1.2.zip
sudo cp index.php Install_Prestashop.html prestashop.zip /var/www/html
sudo chown -R apache: /var/www/html/*
```

## Résultat

Installation de Prestashop fonctionnelle

**Procédure à utiliser pour Docker**


 
