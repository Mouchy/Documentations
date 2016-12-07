Symfony fosrestBundle
#####################

FOSRestBundle
*************

On peut trouver des exemples sur le fonctionnement rest avec symfony avec le projet ci-dessous. 
C'est un projet pret à télécharger avec un exemple 
https://github.com/gimler/symfony-rest-edition

Download the Bundle ::

 php composer.phar require friendsofsymfony/rest-bundle @stable

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

 # FOSRestBundle
 fos_rest:

    param_fetcher_listener: true
    body_listener: true
    format_listener: true
    view:
        view_response_listener: 'force'
        formats:
            xml: true
            json : true
        templating_formats:
            html: true
        force_redirects:
            html: true
        failed_validation: HTTP_BAD_REQUEST
        default_engine: twig
    routing_loader:
        default_format: json
		
Mise en place dans le code
**************************

J'ai créé un bundle SD\Snowyday. Je rajoute un nouveau controller manuellement LocationsController.php.
Je ne rajoute pas d'inclusion vers le FOSRestBundle car ici je ne vais pas hériter de la classe FOSRestBundle je n'utilise aucune fonction 
de cette classe mon exemple est le plus simple possible. 
Attention à bien remarquer que **je rajoute get** devant le nom de ma fonction getLocationsAction 
cela permet à FOSRestBundle de trouver la fonction ::

 //src/SD/SnowydayBundle/Controller/LocationsController.php
 
 namespace SD\SnowydayBundle\Controller;

 use Symfony\Bundle\FrameworkBundle\Controller\Controller;

 class LocationsController extends Controller
 {
    public function getLocationsAction()
    {
         $identite = array(
          'nom' => 'Hamon', 
          'prenom' => 'Hugo', 
          'age' => 19, 
          'estEtudiant' => true
        );
            
        return array('identite' => $identite);
    }
 }


Dans le fichier routing.yml je rajoute la route vers mon nouveau controller.
Je le type rest cela indique à FOSRestBundle de prendre en charge cette route. ::

 /app/config/routing.yml
 locations:
    type: rest
    resource: SD\SnowydayBundle\Controller\LocationsController

Normalement le nouveau controleur Rest est fonctionnel.
On peut le tester à l'aide de la commande ::

 curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/locations | python -mjson.tool

