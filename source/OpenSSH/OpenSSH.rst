OpenSSH
#######


Connection à une machine distante ::

 ssh login@ip
 ssh oracod@emeagvasun11
 
Executer un script sur une machine distante. Le script étant sur la machine local ::

 ssh root@MachineB 'bash -s' < local_script.txt

Cette commande semble marcher depuis windows




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