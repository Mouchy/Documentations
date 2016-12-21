############
UnixCommands
############

DBX debugger sous unix
***********************
 
Différente commande possible pour lancer le debug en fonction des UNIX 

dbx /inst/exe/ctasrv 

dbx APSYS_EXE/ctasrv 


dbx `which lstsrv`

attach pid_du_process 


dbx - a pid_du_process   (avec cette commande pas besoin de faire le attach) 

dbx /inst/exe/ctasrv pid_du_process 


stop in function 

stop at numero_ligne 

list pour voir ce qui suit lorsque l'on est arrété dans du code 

Examine adress/s       /s pour dire que c'est du type string 

Print variable pour voir le contenu d'une variable une fois 

watch variable pour voir la variable à chaque modification 


Commandes SSH
*************

Accès à distance à la console en ligne de commande (shell ssh)
==============================================================

::

 ssh <nom_utilisateur>@<ipaddress> -p <num_port>

L'option -p <num_port> qui précise le port utilisé par le serveur est facultative. Si rien n'est précisé, c'est le port 22 qui sera utilisé par défaut (protocole TCP). 

Transfert - copie de fichiers
=============================

::

 scp <fichier> <username>@<ipaddressDistant>:<DestinationDirectory>
 
Exemple Pour un fichier ::
  
 scp fichier.txt hornbeck@192.168.1.103:/home/hornbeck
  
Exemple  Pour un répertoire ::

 scp -r répertoire hornbeck@192.168.1.103:/home/hornbeck/
 
Vous pouvez aussi bien copier des fichiers à partir des ordinateurs à distance sur votre disque local ::

 scp hornbeck@192.168.1.103:/home/hornbeck/urls.txt .

Ici, le point . à la fin de commande indique de copier le fichier dans le répertoire courant.

Authentification par un système de clés publique/privée
=======================================================

Sur le client générer une clé publique. ::

 ssh-keygen -t rsa

Cela génére une repertoire .ssh dans le home de l’utilisateur courant
Dans ce repertoire deux fichiers ont été généré,
id_rsa.pub la clé publique à transférer sur le serveur.
id_rsa la clé privé à garder sur le client.

Sur le serveur je n’avais pas de repertoire .ssh j’ai alors executé la commande ssh-keygen -t rsa pour le créer automatiquement (c’est une technique qui vaut ce quelle vaut)

Copie de la clé sur le serveur.
Sur le client se placer dans le repertoire .ssh ::
 
 scp id_rsa.pub manu@192.168.1.39:~

se connecter au serveur ::

 ssh manu@192.168.1.39

sur le serveur ::

 echo $(cat ~/id_rsa.pub) >> .ssh/authorized_keys

Il faut ensuite désactiver la connection par mot de passe

Ouvrir le fichier /etc/ssh/sshd_config avec nano
Changer les ligne comme ci-dessous
- A la ligne PasswordAuthentication mettre no
- A la ligne UsePAM mettre "no"

Relancer
sudo service ssh restart


grep : filtrer des données
**************************

Le premier paramètre est le texte à rechercher, le second est le nom du fichier dans lequel ce texte doit être recherché ::

 grep texte nomfichier
 
 
Notez qu'il n'est pas nécessaire de mettre des guillemets autour du mot à trouver, sauf si vous recherchez une suite de plusieurs mots séparés par des espaces, comme ceci ::

 grep "Site du Zéro" monfichier
 
 -i : ne pas tenir compte de la casse (majuscules / minuscules) ::

 grep -i alias .bashrc
 
-n : connaître les numéros des lignes
 
Find
****
**simple**
dans le repertoire ::

 find monfichier*.*

**avancé** ::

 find /home -name monfichier

recherche tous les fichiers ayant une extension .c ::

 find . -name "*.c" 

recherche les fichiers du répertoire courant qui ont été modifiés entre maintenant et il y a 5 jours ::

 find . -mtime -5 

recherche uniquement les fichiers (! -type d signifie n'est pas un répertoire) ayant été modifiés ces dernières 24h ::

 find /home/ -mtime -1 \! -type d 

affiche tous les fichiers n'appartenant pas à l'utilisateur root ::

 find . ! -user root 

recherche et supprime tous les fichiers WMA et WMV trouvés :: 

 find . \( -name '*.wmv' -o -name '*.wma' \) -exec rm {} \;


Groupe
******

id -Gn [user]
-G, --groups - print all group IDs
-n, --name - print a name instead of a number, for -ugG
-p - Make the output human-readable.



