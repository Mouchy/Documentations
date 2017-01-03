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


surveillance réseau
*******************

Commande pour surveiller le ping sur la machine ::

 tshark -n -i eth0 -f icmp
	
Routage
*******
	
Description du routage
======================

Sur le routeur ou futur routeur on peut effectuer les commandes ci-dessous.

Demander la table de routage actuelle ::

 route -n

Pour arrêter le routage, il suffit d’entrer la commande ::

 echo 0 > /proc/sys/net/ipv4/ip _ forward

Pour vérifier dans quel état se trouve le routeur (actif ou
inactif, il suffit de consulter ce fichier ::

 # cat /proc/sys/net/ipv4/ip _ forward

Maintenant, nous activons le routage ::

 echo 1 > /proc/sys/net/ipv4/ip _ forward

il va falloir activer la translation d’adresses ::

 # /sbin/iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

 
Firewall
********

Par défaut, le firewall n’est pas actif. Aucune règle n’est
active, et la politique sur chacune des chaînes est à ACCEPT.
Pour purger les règles du firewall, il faut utiliser la commande ci-dessous, appliquée à la table par défaut (filter)  et à
la table NAT ::

 iptables -F
 iptables -F -t nat

Pour vérifier l’état des règles et des politiques,
il faut utiliser ::

 iptables -L
 iptables -L -t nat

Si une politique n’est pas à ACCEPT, il est possible de
changer par la commande suivante ::

 iptables -P FORWARD ACCEPT