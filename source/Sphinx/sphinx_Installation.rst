.. versionadded:: 1.0
    1 février 2013
.. versionadded:: 1.1
    The linux sphinx installation

sphinx installation
===================

**Installation Windows derriére un firewall d'entreprise ne permettant pas l'accés à internet autre que le browser internet**

1. Telecharger Pyton et l'installer http://www.python.org/ftp/python/3.3.0/python-3.3.0.msi

#. Ajouter le repertoire d'installation de python et le repertoire pyton/scripts au path windows

#. Telecharger le setuptools pour python il se trouve dans la partie package du site de python
	http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11.win32-py2.7.exe#md5=57e1e64f6b7c7f1d2eddfc9746bbaf20

#. Telecharger docutils sur le site git ci-dessous
	http://repo.or.cz/w/docutils.git
	Decompresser le fichier dans un repertoire autre que celui de python
	Dans une fenetre dos aller dans le repertoire ou l'on a décompresser le fichier et dans le repertoire docutils
	et éxecuter la commande 
   
	``setup.py install``
   
   à essayé aussi lors d'une prochaine install pour voir si ça marche
   
   ``easy_install nomdufichier.zip``

#. Telecharger JinJa2 
	https://github.com/mitsuhiko/jinja2

	Decompresser le fichier dans un repertoire autre que celui de python
	Dans une fenetre dos aller dans le repertoire ou l'on a décompresser le fichier et dans le repertoire docutils
	et éxecuter la commande
    
	``setup.py install``
   
     à essayé aussi lors d'une prochaine install pour voir si ça marche
   
   ``easy_install nomdufichier.zip``
      

#. Télécharger pigment il faut essayer de le telecharger au format zip
	http://pypi.python.org/pypi/Pygments
	Renommer le fichier télécharge en pygments.egg (peut être pas nécessaire lors d'une prochaine install il faudrait essayé de voir sans le renommer) 
	Dans une fenetre DOS
	aller dans le repertoire ou se trouve le fichier
	
   ``easy_install pigments.egg``

#. Ajout dernière version 
Télécharger MarkupSafe https://pypi.python.org/pypi/MarkupSafe/0.23 version tar.gz
et éxecuter la commande 

``easy_install MarkupSafe-0.23.tar.gz``

#. Telecharger sphinx il faut essayer de le telecharger au format zip, pour les dernière version utiliser plutot le format tar.gz

   Renommer le fichier télécharge en sphinx.egg (peut être pas nécessaire lors d'une prochaine install il faudrait essayé de voir sans le renommer) 
	Dans une fenetre DOS
	aller dans le repertoire ou se trouve le fichier:
	
   ``easy_install sphinx.egg``
   
   Ou utiliser la commande suivante
   
   ``easy_install Sphinx-1.2.2.tar.gz``


Python 3.3
1. Telecharger Pyton et l'installer http://www.python.org/ftp/python/3.3.0/python-3.3.0.msi






**Installation Linux**

1. Installation Python
	Sur la distribution Ubuntu Python semble installer de base

#. Installation docutils
	Télécharge  le tar.gz
	Le décompresser dans un repertoire autre python
	ce rendre dans le repertoire dans lequel on vient de décompresser docutils dans une fenétre avec une prompt

``sudo python setup.py install``

#. Installation JinJa2
	se connecter en tant que root

		``sudo -i``
			``easy_install jinja2``


#. Installation pygment
	se connecter en tant que root

		``sudo -i``
			``sudo easy_install Pygments``
	
#. Installation sphinx
	se connecter en tant que root
		
      ``sudo -i``
			``sudo easy_install sphinx``
         
         
**Installation plugin sphinx avec Eclipse**
Il faut avoir installer sphinx(faire l'installation comme ci-dessus en fonction de sa machine).
Ajouter le repository contenant le plugin sphinx.
Dans le menu eclipse selectionner Help->Install new software.
Une fenêtre d'installation s'ouvre cliquer sur Add.
Dans le champ name mettre: reStructuredText.
Dans le champ location mettre: http://resteditor.sourceforge.net/eclipse.
Dans la combobox work with selectionné le repository que l'on vient d'ajouter "reStructuredText"
Cocher ReST Editor Plugin.
En fonction de la distribution eclipse choisie l'installation se passe plus ou moins bien.
Normalement il faut avoir la distribution Eclipse IDE for java Developpers pour pouvoir 
tout installer si ce n'est pas le cas Eclipse Color Theme plugin risque de ne pas s'installer.


reStructuredText - http://resteditor.sourceforge.net/eclipse         
	
**Configuration de sphinx**	
	
Setting up the documentation sources
	
   ``sphinx-quickstart``

	Cela génére le repertoire pour le projet et aussi un fichier make (linux) et un fichier make.bat (windows)
	Cela généer aussi le fichier index.rst avec le contenu ci-dessous

	TODO

  
 

