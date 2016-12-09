installation de git
###################

sudo apt-get install git-core

Création d'un dépot
mkdir /home/USER_NAME/gitdepot/fbudget
cd gitdepot\fbudget
git init --bare

2)

Installation d'un serveur APACHE
On a plusieurs solution soit installer apache en passant par les depots Ubuntu 
ou bien installer un serveur tout prèt comme XAMPP


2.1) Installation via les dépots Ubuntu

sudo apt-get install apache2 

Apache est installé dans le repertoire
/etc/apache2

Pour activer le module rewrite
sudo a2enmod rewrite

On redémarre Apache2
sudo service apache2 restart

3) gitweb
sudo apt-get install gitweb
L'installation a généré
/usr/share/gitweb qui contient les données du site lui même

/usr/lib/cgi-bin/gitweb.cgi

/etc/gitweb.conf       
contient le repertoire d'install de notre repertoire git
$projectroot = "/home/multimedia/gitdepot

/etc/apache/conf.d/gitweb
Alias /gitweb /usr/share/gitweb
<Directory /usr/share/gitweb>
Options FollowSymlinks +execCGI
AddHandler cgi-script .cgi
</Directory>

On va créer un nouveau site  gitserver

Dans le repertoire /var/www créer un nouveau répertoire
sudo mkdir gitserver

Dans ce repertoire on va installer le site
Ici aussi 2 solutions soit recopier les diférents composant soit faire des liens vers notre site
comme ça lors d'une mise à jour on a rien à faire

On créer donc un lien depuis le repertoire gitweb vers notre serveur
sudo ln -s /usr/share/gitweb /var/www/gitserver
sudo ln -s /usr/lib/cgi-bin /var/www/gitserver/cgi-bin

Déclarer le nouveau site dans apache
Se rendre dans le repertoire
/etc/apache2/sites-available
créer un nouveau fichier gitserver
sudo nano gitserver
<VirtualHost *:80>
ServerName gitserver
Documentroot /var/www/gitserver
<Directory "/var/www/gitserver">
Allow from All
Option +ExecCGI
AllowOverride All
AddHandler cgi-script cgi
DirectoyIndex /cgi-bin/gitweb.cgi
</Directory>

SetEnv GIT_HTTP_EXPORT_ALL
ScriptAlias /git/ /usr/lib/git-core/git-http-backend/
</VirtualHost>


Il faut maintenant activer le vhost. Pour cela il faut créer un lien symbolique du fichier de sites-available/ vers sites-enabled/.
En utilisant:

sudo a2ensite nomduvhost
sudo a2ensite gitserver

Normalement on doit maintenant pouvoir se connecter à gitweb 
Dans un browser taper gitserver 


