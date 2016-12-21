Nouveau projet git dans eclipse
###############################

Pour utiliser git dans eclipse deux possibilites. Soit on initialise le projet git à partir d'un projet deja existant dans Eclipse.
Bouton droit sur le projet que l'on souhaite initialiser avec git puis l'option Team et puis l'option share project.

.. image:: images/Eclipsegit.png

Une fenetre d'option s'ouvre pour proposer les differents CVS disponibles si Egit est installe alors on pourra le partager avec git.

.. image:: images/Eclipsegit4.png


Sinon cloner un projet existant depuis un repository git.
Pour cela faire File->import

.. image:: images/Eclipsegit2.png

.. image:: images/Eclipsegit3.png

J'utilise l'option Projects from git.

.. image:: images/Eclipsegit5.png


On remplit maintenant le detail du protocol selectionné pour cloner le dépot distant.
Dans le cas ci-dessous j'utilise ssh (à confirmer que toutes les valeurs saisie fonctionnent notament l'URI).
Le user/password il me semble n'est pas utile lorsque je suis configuré avec connection avec un fichier comme clé.

.. image:: images/Eclipsegit6.png
 
Exemple pour github en https

.. image:: images/Eclipsegit7.png

Puis on configure le repertoire dans lequel on va cloner le projet et en même temps initialiser le repository sur notre machine.

.. image:: images/Eclipsegit8.png

A cette étape on choisit le repertoire ou l'on souhaite cloner le repository distant.
Automatiquement le repertoire zf2 va être créer. je choisie simplement le repertoire ou je souhaite créer le repertoire zf2.
 
.. image:: images/Eclipsegit9.png

Puis on va créer le projet Eclipse par exemple pour nous un projet PHP dans lequel on importera plus tard la librairie ZendFramework.

.. image:: images/Eclipsegit10.png

.. image:: images/Eclipsegit11.png

.. image:: images/Eclipsegit12.png

.. image:: images/Eclipsegit13.png


Ici mon repertoire de travail existe déjà Z:\\xampp_zf2\\htdocs\\zf2 il a été créé au moment du clone du projet. 
je peux donc choisir de copier les librairies maintenant ou plus tard si je décide de les télécharger via composer.

Ici pour l'exemple je les copie immédiatement

.. image:: images/Eclipsegit16.png


Puis j'indique simplement le repertoire ou se situe cette librairie à l'aide du bouton add external source folder


.. image:: images/Eclipsegit14.png 

.. image:: images/Eclipsegit17.png

.. image:: images/Eclipsegit18.png

Et voilà projet créé depuis un repository git


Si l'on souhaite rajouter une librairie par la suite.
Bouton droit de la souris sur le projet puis properties

.. image:: images/Eclipsegit19.png

Puis includ_path dans les options de PHP et finalement onglet library.


.. image:: images/Eclipsegit20.png


Validation des modifications
****************************




