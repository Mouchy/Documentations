RegEX
#####

Il existe deux types d'expressions régulières, qui répondent aux doux noms de :

**POSIX** : c'est un langage d'expressions régulières mis en avant par PHP, qui se veut un peu plus simple que PCRE (ça n'en reste pas moins assez complexe). 
Toutefois, son principal et gros défaut je dirais, c'est que ce « langage » est plus lent que PCRE ;


**PCRE** : ces expressions régulières sont issues d'un autre langage (le Perl). 
Considérées comme un peu plus complexes, elles sont surtout bien plus rapides et performantes.

Les fonctions qui nous intéressent

Nous avons donc choisi PCRE. Il existe plusieurs fonctions utilisant le « langage PCRE » ::

 preg_grep ;
 preg_split ;
 preg_quote ;
 preg_match ;
 preg_match_all ;
 preg_replace ;
 preg_replace_callback.

**preg_match**

Exemple ::

 <?php
    if (preg_match("** Votre REGEX **", "Ce dans quoi vous faites la recherche"))
    {    
      echo 'Le mot que vous cherchez se trouve dans la chaîne';
    }
    else {    
      echo 'Le mot que vous cherchez ne se trouve pas dans la chaîne';
    }
 ?>
 
 

**^** (accent circonflexe) : indique le début d'une chaîne ;

**$** (dollar) : indique la fin d'une chaîne.

 
Les classes de caractères
=========================

**[..]** Entre crochets, c'est ce qu'on appelle une classe de caractères. Cela signifie qu'une des lettres à l'intérieur peut convenir.

Exemple ::
 
 #gr[ioa]s#
 
Les intervalles de classe
*************************
Exemple ::

 [a-z]  Cette phrase contient une lettre en minuscule
 
 [a-zA-Z0-9] La phrase peut contenir une lettre en minuscule, majuscule ou un chiffre. 
 
 #[^0-9]# la phrase ne peut pas contenir de chiffre
 
 #^[^a-z]# Cette phrase ne commence pas par une minuscule
 
Les quantificateurs
===================

Les symboles les plus courants


**?** (point d'interrogation) : ce symbole indique que la lettre est facultative. Elle peut y être 0 ou 1 fois.

Ainsi, #a?# reconnaît 0 ou 1 « a » 

**+** (signe plus) : la lettre est obligatoire. Elle peut apparaître 1 ou plusieurs fois.


Ainsi, #a+# reconnaît « a », « aa », « aaa », « aaaa », etc. ;

***** (étoile) : la lettre est facultative. Elle peut apparaître 0, 1 ou plusieurs fois.

Exemple ::

 Ainsi, #a*# reconnaît « a », « aa », « aaa », « aaaa », etc. Mais s'il n'y a pas de « a », ça fonctionne aussi !
 
 #bor?is# Ce code reconnaîtra « boris » et « bois » !
 
 #Ay(ay)*# Ce code reconnaîtra « Ay », « Ayay », « Ayayay », « Ayayayay »

Vous pouvez utiliser le symbole « | » dans les parenthèses. La regex #Ay(ay|oy)*# renverra par exemple vrai pour « Ayayayoyayayayoyoyoyoyayoy » !
C'est le « ay » OU le « oy » répété plusieurs fois, tout simplement !

Être plus précis grâce aux accolades
====================================

Il y a trois façons d'utiliser les accolades.

{3} : si on met juste un nombre, cela veut dire que la lettre (ou le groupe de lettres s'il est entre parenthèses) doit être répétée 3 fois exactement.
#a{3}# fonctionne donc pour la chaîne « aaa ».

{3,5} : ici, on a plusieurs possibilités. On peut avoir la lettre de 3 à 5 fois.
#a{3,5}# fonctionne pour « aaa », « aaaa », « aaaaa ».

{3,} : si vous mettez une virgule, mais pas de 2e nombre, ça veut dire qu'il peut y en avoir jusqu'à l'infini. Ici, cela signifie « 3 fois ou plus ».
#a{3,} fonctionne pour « aaa », « aaaa », « aaaaa », « aaaaaa »,

Si vous faites attention, vous remarquez que :
• ? revient à écrire {0,1} ;

• + revient à écrire {1,} ;

• * revient à écrire {0,}.

Les métacaractères
==================

Dans le langage PCRE (des regex), les métacaractères qu'il faut connaître sont les suivants :

# ! ^ $ ( ) [ ] { } ? + * . \ |

le problème vous tombe dessus le jour où vous voulez chercher par exemple « Quoi ? » dans une chaîne.

#Quoi ?#

#Quoi \?#

Le cas des classes

Il reste une dernière petite chose à voir (encore un cas particulier), et cela concerne les classes de caractères.
Pas besoin de l'échapper : à l'intérieur de crochets les métacaractères… ne comptent plus !
Ainsi, cette regex marche très bien :
#[a-z?+*{}]# 

3 cas particuliers, cependant.
• « # » (dièse) : il sert toujours à indiquer la fin de la regex. Pour l'utiliser, vous DEVEZ mettre un antislash devant, même dans une classe de caractères.

• « ] » (crochet fermant) : normalement, le crochet fermant indique la fin de la classe. Si vous voulez vous en servir comme d'un caractère que vous recherchez, il faut là aussi mettre un antislash devant.

• « - » (tiret) : encore un cas un peu particulier. Le tiret – vous le savez – sert à définir un intervalle de classe (comme [a-z]). Et si vous voulez ajouter le tiret dans la liste des caractères possibles ? Eh bien il suffit de le mettre soit au début de la classe, soit à la fin. Par exemple : [a-z0-9-] permet de chercher une lettre, un chiffre ou un tiret.

Les classes abrégées
====================

Les classes abrégées ::

 \d Indique un chiffre. Ça revient exactement à taper [0-9]
 \D Indique ce qui n'est PAS un chiffre. Ça revient à taper [^0-9]
 \w Indique un caractère alphanumérique ou un tiret de soulignement. Cela correspond à [a-zA-Z0-9_]
 \W Indique ce qui n'est PAS un mot. Si vous avez suivi, ça revient à taper [^a-zA-Z0-9_]
 \t Indique une tabulation
 \n Indique une nouvelle ligne
 \r Indique un retour chariot
 \s Indique un espace blanc
 \S Indique ce qui n'est PAS un espace blanc (\t \n \r)
 .  Indique n'importe quel caractère. Il autorise donc tout !



 
 
 
 
 