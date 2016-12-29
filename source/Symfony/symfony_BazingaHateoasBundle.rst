Symfony BazingaHateoasBundle
############################

BazingaHateoasBundle
********************

Download the Bundle ::

 composer require willdurand/hateoas-bundle


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