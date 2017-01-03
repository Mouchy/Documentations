Symfony fosrestBundle
#####################

FOSRestBundle
*************

On peut trouver des exemples sur le fonctionnement rest avec symfony avec le projet ci-dessous. 
C'est un projet pret à télécharger avec un exemple 
https://github.com/gimler/symfony-rest-edition

Download the Bundle ::

 php composer.phar require friendsofsymfony/user-bundle "~2.0@dev"

Enable the Bundle ::

 // app/AppKernel.php
 class AppKernel extends Kernel
 {
    public function registerBundles()
    {
        $bundles = array(
            // ...
            new FOS\RestBundle\FOSRestBundle(),
        );

        // ...
    }
 }


JMSSerializerBundle
*******************

Lors de l'installation de ce bundle j'ai eu une erreur. Pour la corriger j'ai décommenté la ligne du fichier app/config/config.yml ::

 serializer:      { enable_annotations: true }

Download the Bundle ::

 php composer.phar require jms/serializer-bundle

Enable the Bundle ::
 
 // app/AppKernel.php
 $bundles = array(
    // ...
    new JMS\SerializerBundle\JMSSerializerBundle(),
    // ... 
 );


DoctrineFixturesBundle
**********************
Ce bundle n'est nécessaire que si il y a des accés bases. Dans les exemples ci-dessous il n'est pas utile.

Download the Bundle ::

 php composer.phar require doctrine/doctrine-fixtures-bundle dev-master

Enable the Bundle ::

 // app/AppKernel.php
 $bundles = array(
    // ...
    new Doctrine\Bundle\FixturesBundle\DoctrineFixturesBundle(),
    // ... 
 );

Configuration de FOSRestBundle
******************************
Pour ce fichier bien faire attention à l'indentation qui doit être identique à celle ci-dessous. ::

 app/config/config.yml 

 sensio_framework_extra:
    view:   { annotations: true }
    router: { annotations: true }

 fos_rest:
    param_fetcher_listener: true
    body_listener: true
    disable_csrf_role: ROLE_API
    allowed_methods_listener: true
    unauthorized_challenge: "Basic realm=\"Restricted Area\""
    access_denied_listener:
        json: true
        xml: true
        html: true
    view:
        view_response_listener: 'force'
        force_redirects:
            html: true
        formats:
            json: true
            xml: true
    format_listener:
        rules:
            - { path: ^/, priorities: [ json, xml ], fallback_format: json, prefer_extension: true}
        
Test FOSRestBundle niveau 1 
***************************

Je ne rajoute pas d'inclusion vers le FOSRestBundle car ici je ne vais pas hériter de la classe FOSRestBundle je n'utilise aucune fonction 
de cette classe mon exemple est le plus simple possible. 

J'ai créé la classe PHP GreetingsController.php ci-dessous. ::

 use Symfony\Bundle\FrameworkBundle\Controller\Controller;

 class GreetingsController 
 {
    public function helloAction()
    {
         $identite = array(
          'nom' => 'Man', 
          'prenom' => 'Del', 
          'age' => 19, 
          'estEtudiant' => true
        );
            
        return array('identite' => $identite);
    }
 }

J'ai déclaré la route dans routing.yml ::

 greetings:
    type: rest
    resource: SD\SnowydayBundle\Controller\GreetingsController

J'obtiens avec php bin/console debug:router la route ci-dessous ::
 
 hello                      GET      ANY      ANY    /hello.{_format}

ce qui donne ::

 curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/hello

Dans ce cas précis je ne peux pas utiliser de majuscule comme premiére lettre celle-ci est enlever par FOSRestBundle pour une raison que j'ignore.
Je peux déclarer autant de fonctions que je le souhaite mais elles seront de type get.
Il est possible qu'il y ait un risque de collision avec les noms des méthodes et une  autre classe.
Si je souhaite changer le type GET de la fonction je dois préfixer celle ci avec la protocole que je souhaite utiliser voir exemple ci-dessous.

Test FOSRestBundle niveau 2 
***************************

J'ai créé un bundle SD\Snowyday. Je rajoute un nouveau controller manuellement GreetingsController.php.
Attention à bien remarquer que **je rajoute get** devant le nom de ma fonction getHelloAction.
C'est un formatage obligatoire. 
On peut noter que la premiére lettre du nom de ma fonction aprés le protocole GET/PUT/ ... doit maintenant être en majuscule.
cela permet à FOSRestBundle de trouver la fonction ::

 //src/SD/SnowydayBundle/Controller/GreetingsController.php
 
 namespace SD\SnowydayBundle\Controller;

 use Symfony\Bundle\FrameworkBundle\Controller\Controller;

 /****************************************************************************/
 /* php bin/console debug:router                                             */
 /*  get_hello                  GET      ANY      ANY    /hello.{_format}    */
 /*  put_hello                  PUT      ANY      ANY    /hello.{_format}    */
 /* curl -X PUT -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/hello*/
 /****************************************************************************/

 class GreetingsController 
 {
    public function getHelloAction()
    {
         $identite = array(
          'nom' => 'getHelloAction', 
          'Protcole' => 'GET', 
          'age' => 19, 
          'estEtudiant' => true
        );
            
        return array('identite' => $identite);
    }
    
     public function putHelloAction()
    {
         $identite = array(
          'nom' => 'putHelloAction', 
          'Protcole' => 'PUT', 
          'age' => 19, 
          'estEtudiant' => true
        );
            
        return array('identite' => $identite);
    }
 }


Dans le fichier routing.yml je rajoute la route vers mon nouveau controller. 
Qui ne change pas par rapport à l'exemple ci-dessus.
Je le type rest cela indique à FOSRestBundle de prendre en charge cette route. ::

 /app/config/routing.yml
 greetings:
    type: rest
    resource: SD\SnowydayBundle\Controller\GreetingsController

Normalement le nouveau controleur Rest est fonctionnel.
On peut le tester à l'aide de la commande ::

 curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/locations | python -mjson.tool

 
Test FOSRestBundle niveau 3: ClassResourceInterface
***************************************************

L'interface ClassResourceInterface permet de rajouter le nom de notre controller dans le chemin de l'adresse URL. ::

 <?php

 namespace SD\SnowydayBundle\Controller;

 use Symfony\Bundle\FrameworkBundle\Controller\Controller;
 use FOS\RestBundle\Routing\ClassResourceInterface;

 /****************************************************************************/
 /* php bin/console debug:router                                             */
 /* get_greetings_greetings    GET      ANY      ANY    /greetings/greetings.{_format} */
 /* put_greetings_hello        PUT      ANY      ANY    /greetings/hello.{_format} */
 /* curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/greetings/greetings */
 /* curl -X PUT -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/greetings/hello */
 /****************************************************************************/

 class GreetingsController implements ClassResourceInterface
 {
    public function getGreetingsAction()
    {
         $identite = array(
          'nom' => 'getHelloAction', 
          'Protcole' => 'GET', 
          'age' => 19, 
          'estEtudiant' => true
        );
            
        return array('identite' => $identite);
    }
    
     public function putHelloAction()
    {
         $identite = array(
          'nom' => 'putHelloAction', 
          'Protcole' => 'PUT', 
          'age' => 19, 
          'estEtudiant' => true
        );
            
        return array('identite' => $identite);
    }
 }

L'exemple ci-dessous donne donc comme route ::

 php bin/console debug:router

 get_greetings_greetings    GET      ANY      ANY    /greetings/greetings.{_format}     
 put_greetings_hello        PUT      ANY      ANY    /greetings/hello.{_format}

On voit que le nom de la classe est intégré dans le chemin de l'adresse URL ::

 curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/greetings/greetings
 curl -X PUT -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/greetings/hello


Test FOSRestBundle niveau 3: FOSRestBundleAnnotations\View()
************************************************************
On ajoute une couche entre le controller et la generation de l'output.
Dans l'exemple ci-dessous on ne genere plus un tableau que l'on renvoie en retour de la fonction mais une classe.

Pour cela on rajoute ::

 use FOS\RestBundle\Controller\Annotations as FOSRestBundleAnnotations;

Et avant la définition de la classe on ajoute notre annotation ::

 /**
  * @FOSRestBundleAnnotations\View()
  */

  Cela donne le code ci-dessous et ensuite on va creer la classe correspondante. ::

 <?php

 namespace SD\SnowydayBundle\Controller;

 use Symfony\Bundle\FrameworkBundle\Controller\Controller;
 use FOS\RestBundle\Routing\ClassResourceInterface;
 use FOS\RestBundle\Controller\Annotations as FOSRestBundleAnnotations;
 use  SD\SnowydayBundle\Entity\Hello;

 /****************************************************************************/
 /* php bin/console debug:router                                             */
 /* hello_greetings            GET      ANY      ANY    /greetings/hello.{_format} */
 /* curl -X PUT -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/greetings/hello */
 /****************************************************************************/

 /**
  * @FOSRestBundleAnnotations\View()
  */

 class GreetingsController implements ClassResourceInterface
 {
    public function helloAction()
    {
        return new Hello();
    }
 }

On va maintenant créer la classe correspondante. J'ai rajouté cette classe dans le repertoire entity de mon bundle.
Cela m'évite de créer une ligne pour l'autoloader ::

 <?php

 namespace SD\SnowydayBundle\Entity;

 /**
  * Class Hello
  */
 class Hello
 {
    /**
     * @var string
     */
    private $greet;

    /**
     * Hello constructor.
     */
    public function __construct()
    {
        $this->greet = "Hello World!!!";
    }

    /**
     * @return string
     */
    public function __toString()
    {
        return $this->greet;
    }
 }