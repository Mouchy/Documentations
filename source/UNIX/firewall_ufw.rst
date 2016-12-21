Firewall ufw
############

source https://sudovm.wordpress.com/2013/04/03/how-to-set-up-the-firewall-using-ufw-on-ubuntu-10-04-lucid-lynx-server/

UFW se base sur ipfilter

autoriser UFW ::

 sudo ufw enable

Arréter UFW ::

 sudo ufw disable

Contrôler la configuration ::

 sudo ufw status
 sudo ufw status verbose

Ajouter des règles
******************

Avant d'ajouter de nouvelles regle on commence par tout bloquer en entree et en sortie. 
Bien evidement pour faire cela il faut etre devant la machine que l'on bloque et non pas en remote ::

 sudo ufw default deny incoming
 sudo ufw default deny outgoing
 
UFW pour un ensemble de regles predefinie ::

 sudo ufw app list

Ouvrir le port ssh (port 22)::

 sudo ufw allow ssh

Ouvrir le port web (port 80) ::

 sudo ufw allow www
 
Ouvrir le port pour samba ::

 sudo ufw allow Samba
 
Ouvrir le port samba uniquement pour son reseau interne ::

 sudo ufw allow from 192.168.1.1/100 to 127.0.0.1 app Samba
 sudo ufw allow to   192.168.1.1/100 from 127.0.0.1 app Samba
 
Autoriser un port ou une plage de port. Il faut dans ce cas specifier si l'on ouvre en UDP ou TCP ::

 sudo ufw allow 9091
 sudo ufw allow 20500:20599/tcp
 sudo ufw allow 20500:20599/udp
 
Autoriser une port pour le reseau local. L'exemple ci-dessous concerne MySQL ::

 sudo ufw allow from 10.0.0.0/8 to any port 3306/tcp

Ouvrir Mysql au monde ::

 sudo ufw allow mysql
 
Effacer une regle. Remplacer <...> avec la regle entiere que l'on souhaite effacer ::

 sudo ufw delete <...>
 sudo ufw delete allow ssh
 sudo ufw delete allow 10000

Effacer toutes les regles ::

 sudo ufw reset