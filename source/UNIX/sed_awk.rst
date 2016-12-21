Sed et Awk
##########

Stream EDitor
*************

http://openclassrooms.com/courses/la-commande-sed

sed fonctionne en mode flux, c'est-a-dire que le flux en entree (fichier ou autre) est traite ligne par ligne.

deux facons d'utiliser sed :

* La methode "classique", qui consiste a appliquer la commande sur le flux d'entree, et recuperer le flux de sortie. Par exemple, on applique sed sur un fichier, et on redirige la sortie sur un autre fichier.
* La methode "directe", avec l'option **sed -i**, qui applique la commande directement sur le fichier passe en entree.

synopsis ::

 sed [OPTION]... {script-only-if-no-other-script} [input-file]...

En plus des options et du flux d'entrée, sed reçoit un script. Ce script contiendra toutes les actions à exécuter sur le flux d'entrée. 

Il existe deux manières de passer un script à sed :

* On peut écrire le script directement dans la ligne de commande, avec l'option **sed -e**
* On peut passer à sed un fichier externe (par exemple myscript.sed) contenant le script, avec **sed -f** script-file

Enregistrez ce fichier sous test.txt ::

 Bonjour,

 Ceci est un fichier de test.
 Ici la ligne numéro 4.

 # ceci pourrait être un commentaire
 Ici la ligne numéro 7.

 Au revoir

Lancez : sed '' test.txt ::

 console$ sed '' test.txt
 Bonjour,

 Ceci est un fichier de test.
 Ici la ligne numéro 4.

 # ceci pourrait être un commentaire
 Ici la ligne numéro 7.

 Au revoir

Vous pouvez voir que sed lancé avec un script vide renvoie simplement le contenu du fichier.

Adressage
=========
**num**       le numero de la ligne
sed -n 3p test.txt

**debut-pas** Toutes les n lignes (pas) en partant de début  
sed -n 1~2p fich.txt

**$**
La dernière ligne du dernier fichier fourni en entrée, ou de chaque fichier si les options "-i" ou "-s" ont été spécifiées 
sed -n '$ p' fich.txt

**/exp/** Toutes les lignes mises en correspondance avec l'expression régulière exp
sed -n '/est/p' fich2.txt

**\#exp#** Toutes les lignes mises en correspondance avec l'expression régulière exp en précisant d'utiliser comme délimiteur le caractère "#" (dièse)
en lieu et place du délimiteur par défaut.
sed -n '\#est#p' fich2.txt

**num1,num2** Toutes les lignes comprises entre num1 et num2. Si num2 venait à être inférieur à num1, seul num1 est affiché 
sed -n '3,6 p' fich.txt

**/exp1/,/exp2/** Toutes les lignes comprises entre exp1 et exp2
sed -n '/commentaire1/,/commentaire2/ p' fich2.txt

**num,/exp/** ou **/exp/,num** Toutes les lignes comprises entre un numéro de ligne et une expression régulière (ou l'inverse).
sed -n '2,/commentaire2/ p' fich2.txt

Adressage par ligne
-------------------
sed -e '4d; 7d' test.txt ::

 console$ sed -e '4 d; 7 d' test.txt
 Bonjour,

 Ceci est un fichier de test.

 # ceci pourrait être un commentaire

 Au revoir

Ici, nous utilisons l'adressage par ligne. La commande d (delete) indique que l'on va supprimer la ligne.

L'option -e permet de passer plusieurs commandes à la suite. Je pense que c'est une bonne habitude de l'ajouter de façon systématique.

On peut aussi utiliser l'adressage par intervalle, comme ceci : sed '4,7 d' test.txt

Cette commande va effacer toutes les lignes comprises entre la ligne 4 et la ligne 7 ::

 console$ sed '4,7 d' test.txt
 Bonjour,

 Ceci est un fichier de test.

 Au revoir

Adressage par motif
-------------------

On peut aussi utiliser des regex pour appliquer la commandes sur toutes les lignes ou le motif est trouvé. 

On décrit un motif comme ceci : **/regex/**

Exemple :: 

 sed '/^#/ d' test.txt

supprimera toutes les lignes commençant par une dièse (le ^ est un métacaractère signifiant début de ligne) ::

 console$ sed '/^#/d' test.txt
 Bonjour,

 Ceci est un fichier de test.
 Ici la ligne numéro 4.

 Ici la ligne numéro 7.

 Au revoir

On peut très bien aussi utiliser un adressage mixte, comme ::

 sed '/^#/,7 d' test.txt

ou ::

 sed '4,/^#/ d' test.txt

Mode silencieux
---------------

Il existe une autre façon d'utiliser sed, particulièrement intéressante. 
C'est l'utilisation en mode "silencieux", c'est-à-dire que sed ne doit afficher par défaut aucune ligne. 
Seules les lignes intéressantes seront affichées, avec la commande p (print). Pour passer en mode "silencieux", il faut utiliser l'option ::

 sed -n

Lancez sed -n '/Ici/p' test.txt ::

 console$ sed -n '/Ici/p' test.txt
 Ici la ligne numéro 4.
 Ici la ligne numéro 7.

A noter que la négation d'adresse est possible. Ainsi, cette commande produira exactement le même résultat ::

 sed '/Ici/!d' test.txt


Les commandes
=============

# Commentaire

q quit quitter (une adresse autorisée) 
sed '3q' fich.txt

d delete effacer (intervalle d'adresse autorisée) 

sed '3d' fich.txt

p print affichage (intervalle d'adresse autorisée) 

sed -n '3{p;q}' fich.txt

n next-line ligne suivante (intervalle d'adresse autorisée) 

{ ... } commandes groupées (intervalle d'adresse autorisée) L'emploi d'accolades permet de regrouper certaines commandes à effectuer sur une adresse ou une plage d'adresses.

echo -e "AAA\nBBB\nCCC\nDDD" | sed -n '/BBB/ {n;s/C/Z/2p}'

s substitution 



sed permet de remplacer du texte avec des regex, syntaxe étendue ::

 sed -r

Substitution
------------

La substitution s'écrit comme ceci ::

 s/modèle/remplacement/drapeau(x) ou 
 s/motif/substitut/

Autant la partie gauche (recherche) accepte la syntaxe des BRE (Basic Regular Expression, expressions régulières basiques),
la partie droite (remplacement) quant à elle n'accepte que trois valeurs pouvant être interpolées : 
le caractère & (esperluette) 
les références arrières \1 (de 1 à 9) 
les options \U,\u,\L,\l et \E

Flag
----
La commande de substitution (s) peut être suivie d'aucun ou de plusieurs drapeaux/attributs 
**g** global Effectue le remplacement de toutes les occurrences mises en correspondance par le motif ou l'expression régulière
**N** nième occurrence Remplace uniquement la nième occurrence mise en correspondance par le motif ou l'expression régulière

**w** fichier - Write (écriture dans un fichier) 

**e** evaluate (évaluation) Permet de faire exécuter une commande par le shell et d'en substituer le résultat avec le motif mis en correspondance, 
uniquement si une correspondance a été établie ::

 sed 's/.*5/echo '$A'/e' 

**I** case-Insensitive Permet d'ignorer la casse lors de la mise en correspondance du motif ::

 sed 's/bONjOUr/Salut/I'

Par défaut, elle s'effectue sur la première occurrence du motif, sauf si on lui ajoute l'option g comme ceci ::

 s/motif/substitut/g

Quelques exemples ::

 sed -re 's/^# *//' fichier

décommente les lignes commentées (commençant par une dièse), 
et supprime les espaces en début de ligne (le * est un métacaractère signifiant 0 ou plus) ::

 sed -re 's/\t/    /g' fichier

remplace les tabulations par 4 espaces.

Translittération
----------------

La translittération permet d'échanger certains caractères avec d'autres caractères.

On l'écrit comme ceci :: 

 y/liste1/liste2/.

Par exemple, pour supprimer les accents sur les e dans notre fichier test.txt, on fera ::

 sed -re 'y/éèê/eee/' test.txt
 console$ sed -re 'y/éèê/eee/' test.txt
 Bonjour,

 Ceci est un fichier de test.
 Ici la ligne numero 4.

 # ceci pourrait etre un commentaire
 Ici la ligne numero 7.

 Au revoir
 
Utilisation avancée
===================
Commandes groupées
------------------
Sed permet de grouper les commandes avec des accolades. Cela permet de faire plus de choses dans une même commande ::

 sed -e '/motif_1/ {commande1; commande2} ; /motif_2/ {commande1; commande2; /sous_motif/ {commande1; commande2} }

Utilisation multiligne
----------------------
La commande N
^^^^^^^^^^^^^

sed -e 'N; s/\n//' test.txt

La commande D
^^^^^^^^^^^^^
La commande d (delete) dispose aussi d'un équivalent spécialisé pour le traitement multiligne.
Exemple d'utilisation : si l'on veut par exemple supprimer les lignes vides d'un fichier, on ne peut pas le faire comme ceci :: 

 sed -re '/^$/ {N; s/\n//}' 

fichier, à cause de la particularité évoquée ci-dessus. En effet, si nous avons plusieurs lignes vides à la suite, certaines vont rester.

On pourra par contre le faire comme cela ::

 sed -re '/^$/ {N; D}' fichier

Labels et branchements
^^^^^^^^^^^^^^^^^^^^^^

Une autre fonctionnalité importante et qui ajoute énormément de puissance à sed est la possibilité de branchement dans le script. 
Il est possible en effet de revenir en arrière dans le script, en créant des labels.

Un label s'écrit :label.

Supposons que vous ayez devant vous un fichier HTML, et que vous voulez supprimer toutes les balises qu'il contient et ne garder que le texte.

Pour faire cela, il y a la commande b (branch). :)

Voici un script qui marche même avec des balises sur plusieurs lignes ::

 sed -re ':start s/<[^>]*>//g; /</ {N; b start}' fichier.html




AWK
***

