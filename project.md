Welcome to the Wikinets Research wiki!

Hello! This is my contribution for the Linux final project.
Yasir found a great website with full documentation of LAMP as well as Mediawiki installation. I am still having trouble with renaming the URL from localhost which is under Step 3 on the link to install Mediawiki for Ubuntu. When I edit the ServerName, I cannot find the new url in my web browser. Mediawiki is only showing under localhost currently. If any of you can help with this, that would be great!

Here are the Links

How to Install Mediawiki for Ubuntu
and
How to Install LAMP

You have the option to install LEMP as well, I tried both, but found LAMP to be more user friendly for me anyways

Of course not everything went smoothly when following the links above. They were very useful but I found the rest of the information through a youtube video below.

Setup a Wiki - Youtube Tutorial

Here is what I gathered from both resources above to install mediawiki.
Installing LAMP
Step 1: Update Ubuntu

    sudo apt-get update

    sudo apt-get upgrade

    sudo apt-get dist-upgrade

Apache
Step 2: Install Apache

    sudo apt-get install apache2 apache2-utils

now you will want to make sure apache2 is active and running

    systemctl status apache2

if it is not running, enter this command

    sudo systemctl start apache2

then run this command to make sure Apache2 runs automatically after reboot

    sudo systemctl enable apache2

Let's test if its working. Enter localhost into the web address bar. It should show as the image below.

now you can change owner of the web root directory. change www-data to user

    sudo chown www-data /var/www/html/ -R

Mariadb
Step 3: Install Mariadb

Let's install Mariadb and then check to see if it is active

    sudo apt-get install mariadb-server mariadb-client

    systemctl status mysql

if it is not active, you will want to use systemctl again and enable it to run automatically after reboot.

    sudo systemctl start mysql

    sudo systemctl enable mysql

We can now set security for mariadb, read the prompts and carefully select password and settings.

    sudo mysql_secure_installation

PHP
Step 4: Install PHP

    sudo apt-get install php7.0-fpm php7.0-mysql php7.0-common php7.0-gd php7.0-json php7.0-cli php7.0-curl libapache2-mod-php7.0 php-xml

    sudo apt install php-mbstring php-json php-mysql php-curl php-intl php-gd texlive

    sudo apt-get install curl php5-cli git

    sudo apt-get install curl php-cli php-mbstring git unzip

Enable and restart Apache

    sudo a2enmod php7.0

    sudo systemctl restart apache2

MediaWiki
Step 5: Installing Mediawiki

install mediawiki from github from

    git clone https://gerrit.wikimedia.org/r/p/mediawiki/core.git

rename folder core into mediawiki

    mv core mediawiki

now you will want to move the mediawiki folder into the /var/lib folder from within which ever directory you original installed it to (if you have left the directory from which you originally used the git clone command, please return)

    sudo mv mediawiki /var/lib

let's check that mediawiki can now be found in /var/lib

    cd /var/lib

    ls

now lets create a link for mediawiki into /var/www/html

    cd /var/www/html

    sudo ln -s /var/lib/mediawiki mediawiki

and check to see if /var/lib now holds a mediawiki link

    ls

you will now need to install the composer

    sudo apt install composer

    cd /var/lib/mediawiki

    composer install --no-dev

Change web server user as the owner of this directory. www-data on both sides of the : should be owner of directories name.

    sudo chown www-data:www-data /var/www/mediawiki/ -R

Step 6: Creating a Database

log into Mariadb

    mysql -u root -p

You can change wikidb into whatever Database name you'd prefer, I chose Classwiki

    CREATE DATABASE wikidb;

Replace wikidb, wikiuser and password with your preferred database name, database username and user password

    GRANT ALL PRIVILEGES ON wikidb.* TO 'wikiuser'@'localhost' IDENTIFIED BY 'password';

flush and exit

    flush privileges;

    exit;

Step 7: Install Mediawiki on Wed

In the browser tab, enter in the following which should bring you to the page below

    http://localhost/mediawiki

Change settings to match your preferences, when you get to the page below, please enter in the information you gave for steps 6

Continue with Default settings until you reach the Options page. Choose the type of wiki you would like to set up. Once you have chosen, hit continue on to install Mediawiki. It should bring you to a page that will automatically download the LocalSettings.php. Make sure to save this into your Download folder. Then click the link on the page to enter your wiki.

    cd /home/user/Downloads/

run ls to check the file is in Downloads

    ls

next you will want to move the file to /var/lib/mediawiki and check that the file has been moved

    sudo mv LocalSettings.php /var/lib/mediawiki

    cd /var/lib/mediawiki

    ls

We can now download skins

    cd /var/lib/mediawiki/skins/

    sudo git clone https://gerrit.wikimedia.org/r/mediawiki/skins/Vector

You will now have to edit the LocalSettings.php folder to enable skins

    sudo nano /var/lib/mediawiki/LocalSettings.php

copy this to the end of the 'LocalSettings.php` file

    wfLoadSkin( 'Vector' );

Save and close the file then refresh your wikipage. You should be able to log in now.
