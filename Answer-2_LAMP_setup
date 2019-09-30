#!/bin/bash

sudo apt update

sudo apt install apache2 mysql-server php php-mysql libapache2-mod-php php-cli

sudo systemctl enable apache2

sudo systemctl start apache2

sudo chmod -R 0755 /var/www/html/

sudo echo "<?php phpinfo(); ?>" > /var/www/html/info.php

xdg-open "http://localhost"
xdg-open "http://localhost/info.php"

clear
echo "Installing Wordpress Script"
echo "Database Name: "
read -e db
echo "Database User: "
read -e username
echo "Database Password: "
read -s passwrd
echo "run install? (y/n)"
read -e run
if [ "$run" == n ] ; then
exit
else
echo "Setting up wordpress"
curl -O https://wordpress.org/latest.tar.gz			#downloading wordpress
tar -zxvf latest.tar.gz						#unzip wordpress zip file

cd wordpress
cp -rf . ..
cd ..
rm -R wordpress							#removing unwanted files
cp wp-config-sample.php wp-config.php				#creating wp configs

perl -pi -e "s/database_name_here/$db/g" wp-config.php		#adding db name in wp-config.php
perl -pi -e "s/username_here/$username/g" wp-config.php		#adding db username in wp-config.php
perl -pi -e "s/password_here/$passwrd/g" wp-config.php		#adding db passwrd in wp-config.php

#configuring wordpress salts
perl -i -pe'
  BEGIN {
    @chars = ("a" .. "z", "A" .. "Z", 0 .. 9);
    push @chars, split //, "!@#$%^&*()-_ []{}<>~\`+=,.;:/?|";
    sub salt { join "", map $chars[ rand @chars ], 1 .. 64 }
  }
  s/put your unique phrase here/salt()/ge
' wp-config.php

#create uploads folder and set permissions
mkdir wp-content/uploads
chmod 775 wp-content/uploads
echo "Cleaning..."

rm latest.tar.gz						#delete zip file
rm wp.sh							#remove script from server

echo "Installation Complete and MySQL credentails for wordpress are:"
echo "DB username:" $username
echo "DB password:" $passwrd
fi
