Configurer routeur
##################

https://doc.ubuntu-fr.org/routage

Installation d'un réseau
************************
associer une adresse IP à une des interfaces de votre système ::

 sudo ifconfig eth0 add 190.1.1.173

Pour vérifier votre association tapez ::

 ifconfig

.. IMPORTANT::
    Si vous voulez que Ubuntu se souvienne de votre association au redémarrage de l'ordinateur, il vous faut modifier le fichier /etc/network/interfaces, en mettant la commande up suivie de votre nouvelle route.

.. IMPORTANT::
    Pour que les paquets puissent aller d'une carte réseau à une autre, il est indispensable d'activer le forward.
    https://doc.ubuntu-fr.org/partage_de_connexion_internet#avec_le_transfert_d_ip

Description du routage
**********************

Demander la table de routage actuelle ::

 route -n