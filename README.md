# Deploy-Laravel-Project-on-Github-to-Ubuntu-VPS-or-Server---Install-Laravel-PHP-Framework-on-Ubuntu
Deploy Laravel Project on Github to Ubuntu VPS or Server - Install Laravel PHP Framework on Ubuntu

Step by Step in here: https://youtu.be/Xre7YKJnAj8

sudo apt-get update 
sudo apt-get upgrade

sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install apache2 libapache2-mod-php7.2 php7.2 php7.2-xml php7.2-gd php7.2-opcache php7.2-mbstring
sudo apt-get install unzip
sudo apt-get install zip

- Edit PHP.ini: /etc/php/7.2/apache2/php.ini 
memory_limit = 256M
upload_max_filesize = 64M
cgi.fix_pathinfo=0

- Install composer and download Laravel on Ubuntu
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
sudo chmod +x /usr/local/bin/composer

cd /var/www/html
git clone https://github.com/rappasoft/laravel-5-boilerplate.git

cd /var/www/html/laravel-5-boilerplate
sudo composer install

chown -R www-data.www-data /var/www/html/laravel-5-boilerplate
chmod -R 755 /var/www/html/laravel-5-boilerplate
chmod -R 777 /var/www/html/laravel-5-boilerplate/storage

- Generator APP_KEY=base64 for Laravel
mv .env.example .env
php artisan key:generate

- Create Database for Laravel
sudo apt install mariadb-server
sudo mysql_secure_installation       or install mysql: sudo apt-get install mysql-server php7.2-mysql
						     

Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] y
Reload privilege tables now? [Y/n] y

$ mysql -u root -p               (If install mysql then root password is blank)
mysql> CREATE DATABASE laravel;
mysql> GRANT ALL ON laravel.* to 'laravel'@'localhost' IDENTIFIED BY 'secret_password';
mysql> FLUSH PRIVILEGES;
mysql> quit


cd /var/www/html/laravel-5-boilerplate
mv .env 1.env
And edit username, database, password 
mv 1.env .env


Use WinSCP or Cyberduck and access to /etc/apache2/sites-available
rename default conf to laravel.conf

<VirtualHost *:80>
    #ServerName yourdomain.tld

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/laravel-5-boilerplate/public

    <Directory /var/www/html/laravel-5-boilerplate>
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

sudo a2ensite laravel.conf
sudo a2enmod rewrite
sudo service apache2 restart


Import database:
cd /var/www/html/laravel-5-boilerplate
php artisan migrate
php artisan db:seed
php artisan serve

