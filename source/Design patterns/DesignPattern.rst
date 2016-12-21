Design Pattern
##############

https://github.com/bethrobson/Head-First-Design-Patterns/tree/master/src/headfirst/designpatterns

La relation **A-UN**: Lorsque vous assemblez deux classes vous utilisez la composition. 
Au lieu d'hériter leur comportement, les objets l'obtiennent en étant composés avec le bon ojbet comportemental

.. warning:: Principe de conception Préférez la composition à'héritage.

**Le pattern Stratégie** définit une famille d'algorithmes, encapsule chacun d'eux et  les rend interchangeables. 
Stratégie permet à l'algorithme de varier indépendamment des clients qui l'utilisent.

Le **pattern observateur** définit une relation entre objets de type un-à-plusieurs, 
de façon que, lorsque un objet change d'état, tous ceux qui en dépendent 
en soient notifiés et soient mis à jour automatiquement.

Le **pattern Décorateur** attache dynamiquement des responsabilités supplémentaires à un objet. Il fournit une alternative
souple à la dérivation, pour étendre les fonctionnalités.

Le **pattern Fabrication** définit une interface pour la création d'un objet, mais en laissant aux sous-classes le choix des classes à instancier. 
Fabrication permet à une classe de déléguer l'instanciation à des sous-classes.

Le **pattern Fabrique abstraite** fournit une interface pour créer des familles d'objets apparantés ou dépendants sans avoir à spécifier leurs classes concrétes.

Le **pattern Singleton** garantit qu'une classe n'a qu'une seule instance et fournit un point d'accés global à cette instance.

**Base de l'OO** 
::

 Abstraction
 Encapsulation
 Polymorphisme
 Héritage

**Principe OO**
::

 Encapsulez ce qui varient
 Préférerez la composition à l'héritage
 Programmez des interfaces non des implémentations
 Efforcez-vous de coupler faiblement les objets qui interagissent


**Principe de conception**
::

 Efforcez-vous de coupler faiblement les objets qui interagissent.
 Les classes doivent être ouvertes à l'extension mais fermées à la modification.
 Dépendez d'abstractions. Ne dépendez pas de classes concrètes.


