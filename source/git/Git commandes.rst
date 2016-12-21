GIT Commandes
#############

http://rogerdudler.github.io/git-guide/index.fr.html
http://openclassrooms.com/courses/gerez-vos-codes-source-avec-git

Configurer Git
**************

Acitver les couleurs dans git
=============================
Commande git ::

 git config --global color.diff auto
 git config --global color.status auto
 git config --global color.branch auto

Configurer son nom 
==================
Commande git ::

 git config --global user.name "votre_pseudo"

Configurer un e-mail
====================
Commande git ::

 git config --global user.email moi@email.com

Chapitre 2 Git Basics
*********************
Créer un dépôt
==============

Il existe deux façons de créer un dépôt (repository) ::

 **git init** 

créer un dépôt avec les sources à l'endroit ou l'on fait la commande.
Le repertoire contient deux choses un repertoire .git qui contient l'historique des modifications faite. 
Et le repertoire en lui même qui contient nos sources. ::

 **git init --bare** 

créer un dépôt qui contient seulement le repertoire .git d'historique des modification mais aucune source n'est stocké dans le repertoire. 

Se positionner dans le repertoire pour initialiser le projet git ::

 git init
 
Ensuite dans le fichier .git/description il faut mettre une description du projet

Cloner un dépôt
===============

Créer une copie de votre dépôt local ::
 
 git clone /path/to/repository
 
serveur distant ::

 git clone username@host:/path/to/repository

Votre dépôt local est composé de trois arbres géré par git le premier
est votre espace de travail qui contient vos fichiers actuels. Le second est un index qui joue
un rôle d'espace de transit pour vos fichiers et enfin HEAD qui pointe vers la derniére validation 
que vous ayez fait


Vérifier l’état des fichiers
============================

L'outil principal pour déterminer quels fichiers sont dans quel état est la commande et  pour connaitre les fichiers modifier récement ::

 git status


Placer de nouveaux fichiers sous suivis de version
==================================================

Placer de nouveaux fichiers sous suivis de version ou indexer des fichiers modifiers

Ajouter les fichiers que l’on vient de modifier pour être indexé ::
 
 git add <filename>
 
 git add *
 
 Staging Modified Files
 ======================
 
If you change a previously tracked
file called CONTRIBUTING.md and then run your git status command again,
you get something that looks like this ::

 $ git status
 On branch master
 Your branch is up-to-date with 'origin/master'.
 Changes to be committed:
 (use "git reset HEAD <file>..." to unstage)
 new file: README
 Changes not staged for commit:
 (use "git add <file>..." to update what will be committed)
 Recording Changes to the Repository
 
 (use "git checkout -- <file>..." to discard changes in working directory)
 modified: CONTRIBUTING.md
 
The CONTRIBUTING.md file appears under a section named “Changes not
staged for commit” – which means that a file that is tracked has been modified
in the working directory but not yet staged. To stage it, you run the git add
command ::

 $ git add CONTRIBUTING.md

Files are staged and will go into your next commit 


Inspecter les modifications indexées et non indexées - Viewing Your Staged and Unstaged Changes
===============================================================================================

Qu'est-ce qui a été modifié mais pas encore indexé ? Quelle modification a été indexée et est prête pour la validation ? ::

 git diff

Cette commande compare le contenu du répertoire de travail avec la zone d'index. Le résultat vous indique les modifications réalisées mais non indexées.
Il est important de noter que git diff ne montre pas les modifications réalisées depuis la dernière validation — seulement les modifications qui sont non indexées. 
Cela peut introduire une confusion car si tous les fichiers modifiés ont été indexés, git diff n'indiquera aucun changement.


Si vous souhaitez visualiser les modifications indexées qui feront partie de la prochaine validation, vous pouvez utiliser ::

 git diff --cached 

vous pouvez aussi utiliser :: 

 git diff --staged

Qui est plus mnémotechnique. Cette commande compare les fichiers indexés et le dernier instantané :

Valider vos modifications
=========================

Valider les modifications ajouter à l’indexation ::

 git commit 

Avec le commentaire ::

 git commit -m « commentaire »

A partir de là quelques commande sont utiles si l'on ouvre VIM pour commiter les changements

Hit the Esc key; that goes into command mode. Then you can type

* :q to quit (short for :quit)
* :q! to quit without saving (short for :quit!)
* :wq to write and quit (think write and quit)
* :x to write and quit (shorter than :wq)   (j'ai utilisé celle-ci pour écrire mon commentaire)
* :qa to quit all (short for :quitall)


Eliminer la phase d’indexation
==============================

Si vous souhaitez éviter la phase de placement des fichiers dans la zone d'index, Git fournit un raccourci très simple. 
L'ajout de l'option -a à la commande git commit ordonne à Git de placer automatiquement tout fichier déjà en suivi de version dans la zone d'index avant de réaliser la validation,
évitant ainsi d'avoir à taper les commandes git add ::

 git commit -a -m 'added new benchmarks'
 
Les changement sont maintenant dans le HEAD de notre dépôt local pour envoyer les changement au dépôt distant ::

 git push origin master
 
Il est recommandé de créer des tags pour les release des programmes ::
 
 git tag 1.0.0 1b2e1d63ff

le 1b2e1d63ff  désigne les 10 premiers 
caractères de l'identifiant du changement que vous voulez référencer avec le tag on peut obtenir cette identifiant avec la commande ::
 
 git log
   

Viewing the Commit History - Visualiser l'historique des validations
====================================================================

décrire l'historique des branches et fusions
--------------------------------------------
Une des options les plus utiles est -p, qui montre les différences introduites entre chaque validation. Vous pouvez aussi utiliser -2 qui limite la sortie de la commande aux deux entrées les plus récentes ::

 git log -p -2
 
 
Cette option ajoute un joli graphe en caractères ASCII pour décrire l'historique des branches et fusions ::
 
 git log --pretty=format:"%h %s" --graph
 
 Limiter la longueur de l'historique
 -----------------------------------
 --since (depuis) et --until (jusqu'à) sont très utiles ::

 git log --since=2.weeks

git log --oneline --decorate --graph --all

Voir les fichiers déjà indexés
==============================
::

 git ls-files


Working with remote
===================

Showing Your Remotes ::
 
 $git remote
 origin

URLs that Git has stored for the shortname to be used when reading and writing to that remote: ::

 $git remote -v
 origin https://github.com/schacon/ticgit (fetch)
 origin https://github.com/schacon/ticgit (push)

Adding Remote Repositories ::

 git remote add <shortname> <url>
 
 $git remote add pb https://github.com/paulboone/ticgit
 $git remote -v
 pb https://github.com/paulboone/ticgit (fetch)
 pb https://github.com/paulboone/ticgit (push)

Fetching and Pulling from Your Remotes ::

 git fetch [remote-name]
 
It’s important to note that the git fetch command only downloads
the data to your local repository – it doesn’t automatically merge it with any of
your work or modify what you’re currently working on. You have to merge it
manually into your work when you’re ready.

Pushing to Your Remotes ::

 git push [nom-distant] [nom-de-branche]
 $git push origin master


This command works only if you cloned from a server to which you have
write access and if nobody has pushed in the meantime. If you and someone
else clone at the same time and they push upstream and then you push upstream,
your push will rightly be rejected. You’ll have to fetch their work first
and incorporate it into yours before you’ll be allowed to push.

Inspecting a Remote ::

 $ git remote show origin
 * remote origin
 Fetch URL: https://github.com/schacon/ticgit
 Push URL: https://github.com/schacon/ticgit
 HEAD branch: master
 Remote branches:
 master tracked
 dev-branch tracked
 Local branch configured for 'git pull':
 master merges with remote master
 Local ref configured for 'git push':
 master pushes to master (up to date)

Renaming Remotes ::

 $ git remote rename pb paul

Removing Remotes ::

 $git remote rm paul
 
 
 
Branch avec github
==================

Sur Gihub j'ai créé un repository pour me faire des exemples de codes je souhaite garder l'évolution de mon code dans des branch distinctes car cela représente des notions différentes.
Sur github j'ai une branche "master" et je créer une autre branche "ClassResourceInterface".
Puis sur ma machine distantes je fais soit ::

 git remote add origin https://github.com/Mouchy/Album-zf3.git

si je l'ai pas déjà fait. Et si je l'ai déjà fait je fais un ::

 git fetch origin ClassResourceInterface
 
Qui normalement, il me semble me rajoute cette nouvelle branche en local.
Ensuite si je veux faire un push dans cette nouvelle brach je fais ::

 git push -u origin master:ClassResourceInterface

Je ne sais pas si c'est la meilleur méthode mais c'est celle que j'ai trouvé pour l'instant.
A noter que si la branche n'existe pas sur le serveur en remote la commande va créer la branche.

Tagging - Étiquetage 
====================
Lister vos étiquettes
---------------------
Lister les étiquettes existantes ::

 git tag

Cette commande liste les étiquettes dans l'ordre alphabétique. L'ordre dans lequel elles apparaissent n'a aucun rapport avec l'historique.


Créer des étiquettes
---------------------
Git utilise deux types principaux d'étiquettes : légères et annotées.

Les étiquettes annotées
Créer des étiquettes annotées est simple avec Git. Le plus simple est de spécifier l'option -a à la commande tag ::

 $ git tag -a v1.4 -m 'my version 1.4'

Les étiquettes légères
Une autre manière d'étiqueter les commits est d'utiliser les étiquettes légères. Celles-ci se réduisent à stocker la somme de contrôle d'un commit dans un fichier, aucune autre information n'est conservée.
Pour créer une étiquette légère, il suffit de n'utiliser aucune des option -a, -s ou -m ::

 git tag v1.4-lw

Étiqueter après coup
--------------------
you forgot to tag the project at v1.2. Specify
the commit checksum (or part of it) at the end of the command 9fceb02 ::

 git tag -a v1.2 -m 'version 1.2' 9fceb02

Partager les étiquettes
-----------------------
Par défaut, la commande git push ne transfère pas les étiquettes vers les serveurs distants. 
Il faut explicitement pousser les étiquettes après les avoir créées localement. 
Ce processus s'apparente à pousser des branches distantes ::

 git push origin [nom-du-tag]

Si vous avez de nombreuses étiquettes que vous souhaitez pousser en une fois, vous pouvez aussi utiliser l'option --tags avec la commande git push. Ceci transférera toutes les nouvelles étiquettes vers le serveur distant ::

 $ git push origin --tags

Chapitre 3 Git branching
************************

Branch in a nutshell
====================

Creating a new branch ::

 $ git branch testing

Switching Branches ::

 $ git checkout testing
 
Si je reviens sur la branche principal, git checkout master, il est important 
de comprendre que tout les fichiers en local sont alors changé pour être mis tel que dans l'etat de la branche master 


Basic Branching and Merging
===========================
Basic Branching
---------------

Branch and checkout ::

 $ git checkout -b iss53

This is shorthand for ::

 $ git branch iss53
 $ git checkout iss53
 
Ajoute et commit les corrections dans la branche iss53 ::
 
 $ git commit -a -m 'fixed the iss53 problem'

Merge de la branche iss53 dans la branche principale, on se positionne sur la branche et on merge ::

 $ git checkout master
 $ git merge iss53

On peut maintenant effacer cette branche ::

 $ git branch -d iss53
 
 Basic Merging
 -------------
 
 Basic Merge Conflicts
 ---------------------


Remote Branches
===============

You can get a full list of remote references explicitly
with ::

 $ git ls-remote [remote]
 or 
 $ git remote show [remote]


Pushing
-------

Tracking branches
-----------------

Pulling
-------



Chapitre 4 Git on server
************************
The Protocols
=============

local protocol ::

 $ git clone /opt/git/project.git
 $ git clone file:///opt/git/project.git
 
If you specify file://, Git fires up the processes that it normally uses to transfer data over a network

To add a local repository to an existing Git project ::

 $ git remote add local_proj /opt/git/project.git



SMART HTTP
----------
The “smart” HTTP protocol operates very similarly to the SSH or Git protocols
but runs over standard HTTP/S ports

DUMB HTTP
---------
the Dumb HTTP protocol is the simplicity of setting it up. Basically,
all you have to do is put a bare Git repository under your HTTP document
root ::

 $ cd /var/www/htdocs/
 $ git clone --bare /path/to/git_project gitproject.git
 $ cd gitproject.git
 $ mv hooks/post-update.sample hooks/post-update
 $ chmod a+x hooks/post-update


The SSH Protocol
----------------

To clone a Git repository over SSH ::

 $git clone ssh://user@server/project.git
 or
 $ git clone user@server:project.git

Travailler avec des dépôts distants
===================================
Créer un lien vers le code source d'un autre projet git pour l'intégrer au notre ::

 git remote add skeleton https://github.com/zendframework/zendskeletonApplication.git
 
Copier les code source du lien ::
 
 git pull skeleton master
 
Créer une branche du skeleton dans notre serveur ::
 
 git branch ext_skeleton 
 
 
Pour résumé:
Le repertoire de travail créé avec **git init** ou **git clone** est ma copie local du projet. 
C'est là ou l'on ajoute ses modifications, on fait ses test sur un projet
Une fois le travail fini on peut ajouter des nouveaux fichier **git add** et/ou les committer **git commit**. 
Et pour pour un dépôt bare **git push**.
Pour mettre à jour ma copie local de mes fichiers **git pull** met à jour mon répertoire de travail avec le modifications des autre développeurs. 




Afficher les dépôts distants
============================

Pour visualiser les serveurs distants que vous avez enregistrés, vous pouvez lancer la commande ::

 git remote

Vous pouvez aussi spécifier -v, qui vous montre l'URL que Git a stockée pour chaque nom court ::

 git remote -v
 
Ajouter des dépôts distants
===========================

Pour ajouter un nouveau dépôt distant Git comme nom court auquel il est facile de faire référence, lancez ::

 git remote add [nomcourt] [url]
 

Récupérer et tirer depuis des dépôts distants
=============================================

si vous voulez récupérer toute l'information mais que vous ne souhaitez pas l'avoir encore dans votre branche, vous pouvez lancer ::

 git fetch [nomcourt]

Il faut noter que la commande fetch tire les données dans votre dépôt local mais sous sa propre branche — 
elle ne les fusionne pas automatiquement avec aucun de vos travaux ni ne modifie votre copie de travail

Si vous avez créé une branche pour suivre l'évolution d'une branche distante, 
vous pouvez utiliser la commande ::

 git pull 
 
qui récupère et fusionne automatiquement une branche distante dans votre branche locale.

Pousser son travail sur un dépôt distant
========================================

Lorsque votre dépôt vous semble prêt à être partagé, il faut le pousser en amont ::

 git push [nom-distant] [nom-de-branche]

Inspecter un dépôt distant
==========================

Si vous souhaitez visualiser plus d'informations à propos d'un dépôt distant particulier ::

 git remote show [nom-distant]

Exemple pour une création avec git init --bare

Se rendre sur la machine qui servira de **serveur** créer un repertoire pour le projet. 
Par exemple /HOME/user/ProjetBare en ligne de commande se positionner dans le repertoire et taper la commande ::

 git init --bare
 
Commencer le projet sur une des machines **cliente**. Créer un projet sur la machine dans le repertoire de son choix::

 git init
 git add file.txt
 git commit -m 'premiere validation'
 
Puis ajouter le projet d'origine si on est sur une communication ssh comme ci-dessous :: 
 
 git remote add origin git@gitserveur:/HOME/user/ProjetBare
 
Sinon si le dossier est sur une serveur reseau comme SAMBA (a tester pas sur que cela marche) ::

 git remote add origin /z/ProjetBare
  
Pour valider les modifications dans la branche principale ::

 git push origin master
 
 
deuxiéme client se positionner sur une autre machine dans le repertoire de son choix et cloner le projet :: 

 git clone /HOME/user/ProjetBare
 
On retrouve les fichiers du projet dans le repertoire ProjetBare dans le repertoire locale du client. 
Faire une modififcation d'un fichier puis ajouter ce fichier dans le client git local pour un versionning local ::
 
  git add file.txt
  
Commiter cette modification sur le client dans git local ::

 git commit -m "Mon deuxième changement"

Puis valider ces modifications dans le serveur git ::

 git push origin master 


Aide mémoire commande git
*************************

Un petit aide-mémoire pour les commandes que j'utilise dans mon projet Git

Pour un projet sur une machine distante en ssh


Machine distante ::

 $git ini
 
Machine local ::

 $git clone manu@machine:\repertoire\git
 $git add fichier
 $git commit -m "mon premier commit


Cette commande nécessite une explication. En creant le projet sur le serveur avec git init je ne dispose pas des droits pour merger directement
ma branche local avec la branche de la machine distante. Je pousse donc mon travail dans une nouvelle branche sur le serveur distant. Et je vais
devoir me connecter sur le serveur distant pour faire le merge de cette branche. 
Dans l'exemple ci-dessous sur la machine d'origine je créer une nouvelle branche anglais.
Si on initialise le projet git avec l'option --bare on n'a pas ce problème
Et on a donc pas à faire les étapes qui suivent le push fera le merge automatiquement si il n'y a pas de conflit. Malgrés tout pour faire le push sur la machine 
distante le user que j'utilise sur la machine distante doit appartenir au groupe sur lequel on a notre server git pour avoir les droit d'écriture ::

 $git push origine master:anglais

Machine distante ::

 $git merge anglais

Pour visualiser les branches ::

 $git branch



Por créer un nouveau dépot git à partir d'un autre dépot sans passer par la commande git clone

Faire un git init dans le repertoire ou l'on souhaite crée notre dépot git
Puis ajout de le dépot distant dans la base git des depots ::

 git remote add origin https://github.com/Mouchy/Album-zf3.git

puis faire un git fetch pour récupérer les données

 git fetch origin


Synchroniser sont projet local avec le depot distant afin de récupérer les derniéres modifications.
Il faut pour cela qu'il n'y ait pas eu de modification sur le projet local sinon il y aura des conflits au merge à gérer à la main ::

 git fetch origin
 git merge origin/master


