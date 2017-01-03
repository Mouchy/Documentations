Symfony BazingaHateoasBundle
############################

BazingaHateoasBundle
********************

Download the Bundle ::

 php composer.phar require willdurand/hateoas-bundle


Enable the Bundle ::

 // app/AppKernel.php
 class AppKernel extends Kernel
 {
    public function registerBundles()
    {
        $bundles = array(
            // ...
           new Bazinga\Bundle\HateoasBundle\BazingaHateoasBundle(),
        );

        // ...
    }
 }
 
BazingaHateoasBundle utilisation
********************************

Ce bundle permet de renvoyer des informations complémentaires au client.
Notament sous la forme de liens. 
Par exemple si je me connecte avec un user je peux proposer en retour un lien sur lien sur les informations de cette utilisateur.