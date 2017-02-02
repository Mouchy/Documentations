symfonycloud9
#############

CLOUD9 PHPMYADMIN
*****************

Sur cloud9 phpmyadmin n'est pas installé par default il faut le rajouter ::

 phpmyadmin-ctl install

puis on peut accéder avec l'adresse ::

 https://[workspacename]-[username].c9users.io/phpmyadmin

Puis on peut faire un login avec root sans mot de passe qu'il faudra bien sur ajouter par la suite.
Il faut peut etre tester cette commande lors de l'installation de PHP7 ci-dessous


CLOUD9-PHP7
***********
Par defaut sur cloud9 on est dans une version 5.x de PHP pour se positionner 
en version 7 faire les commandes ci-dessous. Après ces commandes on n'aura plus accés à PHPMYADMIN.
Tester la commande suivante pour rétablir phpmyadmin phpmyadmin-ctl install
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
 php -r "if (hash_file('SHA384', 'composer-setup.php') === '55d6ead61b29c7bdee5cccfb50076874187bd9f21f65d8991d46ec5cc90518f447387fb9f76ebae1fbbacf329e583e30') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
 php composer-setup.php
 php -r "unlink('composer-setup.php');"

De temps en temps l'install ci-dessous change donc il faut faire bien attention a ce que tout ce soit bien passé sans message d'erreur et sinon allé sur le lien ci-dessous pour éventuellement reprendre les bonnes commandes.
https://getcomposer.org/download/
 
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
 
 mv symfony/{,.} ./ 
 rm -rf symfony 





