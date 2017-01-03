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


Editeur VI
**********

L'éditeur Unix par défaut se nomme vi (visual editor). S'il n'est pas des plus ergonomiques par
rapport à des éditeurs en mode graphique, il a l'avantage d'être disponible et d'utiliser la même
syntaxe de base sur tous les Unix. Chaque Unix propose généralement une syntaxe étendue au-delà
de la syntaxe de base. Pour en connaître les détails : man vi ::

 vi [options] Fichier [Fichier2 ...]

Trois modes de fonctionnement :
1. mode commande : les saisies représentent des commandes. On y accède en appuyant sur « Echap ».
#. mode saisie : saisie de texte classique
#. mode ligne de commande « à la ex » :utilisation de commandes spéciales saisies et se terminant par Entrée. Accès pas la touche « : ».

5.1 Commandes de saisie
=======================
En mode commande

+----------+-------------------------------------------------------------+
| Commande | Action                                                      |
+==========+=============================================================+
| a        | Ajout de texte derrière le caractère actif                  |
+----------+-------------------------------------------------------------+
| A        | Ajout de texte en fin de ligne                              |
+----------+-------------------------------------------------------------+
| i        | Insertion de texte devant le caractère actif                |
+----------+-------------------------------------------------------------+
| I        | Insertion de texte en début de ligne                        |
+----------+-------------------------------------------------------------+
| o        | Insertion d'une nouvelle ligne sous la ligne active         |
+----------+-------------------------------------------------------------+
| O        | Insertion d'une nouvelle ligne au-dessus de la ligne active |
+----------+-------------------------------------------------------------+

5.2 Quitter
===========
1. La commande ZZ quitte et sauve le fichier
#. :q! quitte sans sauver
#. :q quitte si le fichier n'a pas été modifié
#. :w sauve le fichier
#. :wq ou x sauve et quitte

5.3 Déplacement en mode commande
================================
+----------+------------------------------------+
| Commande | Action                             |
+==========+====================================+
| h        | Vers la gauche                     |
+----------+------------------------------------+
| l        | Vers la droite                     |
+----------+------------------------------------+
| k        | Vers le haut                       |
+----------+------------------------------------+
| j        | Vers le bas                        |
+----------+------------------------------------+
| 0 (zéro) | Début de ligne (:0 première ligne) |
+----------+------------------------------------+
| $        | Fin de ligne (:$ dernière ligne)   |
+----------+------------------------------------+
| w        | Mot suivant                        |
+----------+------------------------------------+
| b        | Mot précédent                      |
+----------+------------------------------------+
| fc       | Saut sur le caractère 'c'          |
+----------+------------------------------------+
| Ctrl + F | Remonte d'un écran                 |
+----------+------------------------------------+
| Ctrl + B | Descend d'un écran                 |
+----------+------------------------------------+
| G        | Dernière ligne du fichier          |
+----------+------------------------------------+
| NG       | Saute à la ligne 'n'(:n identique) |
+----------+------------------------------------+

5.4 Correction
==============
+----------+---------------------------------------------------------------------------------+
| Commande | Action                                                                          |
+==========+=================================================================================+
| x        | Efface le caractère sous le curseur                                             |
+----------+---------------------------------------------------------------------------------+
| X        | Efface le caractère devant le curseur                                           |
+----------+---------------------------------------------------------------------------------+
| rc       | Remplace le caractère sous le curseur par le caractère 'c'                      |
+----------+---------------------------------------------------------------------------------+
| dw       | Efface le mot depuis le curseur jusqu'à la fin du mot                           |
+----------+---------------------------------------------------------------------------------+
| d$ (ou D)| Efface tous les caractères jusqu'à la fin de la ligne                           |
+----------+---------------------------------------------------------------------------------+
| dO       | Efface tous les caractères jusqu'au début de la ligne.                          |
+----------+---------------------------------------------------------------------------------+
| dfc      | Efface tous les caractères de la ligne jusqu'au caractère 'c'                   |
+----------+---------------------------------------------------------------------------------+
| dG       | Efface tous les caractères jusqu'à la dernière ligne, ainsi que la ligne active |
+----------+---------------------------------------------------------------------------------+
| D1G      | Efface tous les caractères jusqu'à la première ligne, ainsi que la ligne active |
+----------+---------------------------------------------------------------------------------+
| dd       | Efface la ligne active                                                          |
+----------+---------------------------------------------------------------------------------+

Ces commandes peuvent être répétées. 5Dd supprime 5 lignes.
On peut annuler la dernière modification avec la commande « u ».