symfonycloud9
#############

CLOUD9-PHP7
***********
Par defaut sur cloud9 on est dans une version 5.x de PHP pour se positionner 
en version 7 faire les commandes ci-dessous. Arpés ces commandes on n'aura plus accés à PHPMYADMIN.
Je n'ai pas encore réussi à le réinstaller. ::

 sudo add-apt-repository ppa:ondrej/php
 sudo apt-get update
 sudo apt-get -y purge php5 libapache2-mod-php5 php5 php5-cli php5-common php5-curl php5-gd php5-imap php5-intl php5-json php5-mcrypt php5-mysql php5-pspell php5-readline php5-sqlite
 sudo apt-get autoremove
 sudo apt-get install php7.0
 sudo apt-get install php7.0-mysql
 mysql-ctl start

CLOUD9-SYMFONY-PHP5
*******************
A faire que si on est en php5.x ::

 sudo apt-get update
 sudo apt-get install php5-intl --> already current
 echo 'date.timezone = UTC' --> works

Installation de mysql ::

 mysql-ctl install --> works
 mysql-ctl start --> works

CLOUD9-MYSQL
************
Ajout de l'utilisateur manu comme super utilisateur ::

 GRANT ALL PRIVILEGES ON *.* TO manu@localhost
      IDENTIFIED BY 'xevrod2x' WITH GRANT OPTION;


CLOUD9-COMPOSER
***************
Pour récupérer composer (composer.phar) utiliser les commandes ci-dessous ::

 php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
 php -r "if (hash_file('SHA384', 'composer-setup.php') === 'aa96f26c2b67226a324c27919f1eb05f21c248b987e6195cad9690d5c1ff713d53020a02ac8c217dbf90a7eacc9d141d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
 php composer-setup.php
 php -r "unlink('composer-setup.php');"	  
	  

CLOUD9-SYMFONY
**************
symfony
-------
On récupére le script symfony ::

 php -r "readfile('https://symfony.com/installer');" > symfony

On lance l'installation du projet ::

 php symfony new api.snowyday.org

composer
--------
Pour installer composer voir ci-dessus.
Je préfére l'install avec composer car elle semble plus compléte que l'install symfony.

Creation sur le site symfony ::

 php composer.phar create-project symfony/framework-standard-edition api2

Creation avec le site open class room ::

 php symfony.phar new project_name
 php app/console generate:bundle 

 
 //web/app_dev.php
 // This check prevents access to debug front controllers that are deployed by accident to production servers.
// Feel free to remove this, extend it, or make something more sophisticated.
if (isset($_SERVER['HTTP_CLIENT_IP'])
    || isset($_SERVER['HTTP_X_FORWARDED_FOR'])
    || !(in_array(@$_SERVER['REMOTE_ADDR'], ['127.0.0.1', '::1']) || php_sapi_name() === 'cli-server')
) {
    header('HTTP/1.0 403 Forbidden');
    exit('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
}
 
 
Déplacer le projet créé à la racine ::
 
 mv symfony/{,.} ./ --> works
 rm -rf symfony --> works





