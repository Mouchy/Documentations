symfony fosuserBundle
#####################

fosuserBundle installation
**************************

Download the Bundle ::

 php composer.phar require

Enable the Bundle ::

 // app/AppKernel.php
 class AppKernel extends Kernel
 {
    public function registerBundles()
    {
        $bundles = array(
            // ...
            new FOS\UserBundle\FOSUserBundle(),
        );

        // ...
    }
 }


fosuserBundle utilisation
*************************

Loader du module snowyday/src/SD/SnowydayBundle/Customer/Customer.php dans snowyday/app/autoload.php ajouter le code ::

 $loader->addPsr4("Customer\\", __DIR__.'/../src/SD/SnowydayBundle/Customer', true);

Puis créer la classe Customer.php :: 
 
 <?php
 
 namespace Customer;
 
 use FOS\UserBundle\Model\User as BaseUser;
 use Doctrine\ORM\Mapping as ORM;
 /**
  * @ORM\Entity
  * @ORM\Table(name="fos_user")
  */
 class Customer extends BaseUser
 {

    /**
     * @ORM\Id
     * @ORM\Column(type="integer")
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

   
    /**
     * Customer constructor.
     * @param $id
     */
    public function __construct()
    {
        $this->id = $this->id ? $this->id : uniqid();
        parent::__construct();
    }

 }
 
Puis dans le repertoire entity de notre bundle on va rajouter la classe Customer.php ::

 <?php
 namespace SD\SnowydayBundle\Entity;

 use Customer\Customer as CustomerBase;
 use Doctrine\ORM\Mapping as ORM;

 /**
 * @ORM\Entity
 * @ORM\Table(name="fos_user")
 */
 class Customer extends CustomerBase
 {
 }

On va créer notre base et tables correspondantes. pour la configuration de symfony avec la base voir 

see :ref:`symfony_database`

A partie de la on créer la base de données associé à notre bundle si ce n'est pas déjà fait ::

 php bin/console doctrine:schema:create --dump-sql

L'option --dump-sql permet de voir le code d'insert du sql pour le controler. On enléve cette option pour vraiment créer la base.

Création d'un utilisateur
*************************
Pour créer un utilisateur dans la base ::

 php bin/symfony fos:user:create

Et on saisie les informations que l'on nous demande.


Accés avec REST
***************

On créer un nouveau controleur customer ::

 <?php

 namespace SD\SnowydayBundle\Controller;

 use FOS\RestBundle\Controller\Annotations as FOSRestBundleAnnotations;
 use FOS\RestBundle\Controller\FOSRestController;
 use FOS\RestBundle\Routing\ClassResourceInterface;
 
 /**
  * @FOSRestBundleAnnotations\View()
  */
 class CustomersController extends FOSRestController implements ClassResourceInterface
 {
    public function cgetAction()
    {
        $em = $this->getDoctrine()->getEntityManager();
        $repository = $em->getRepository("SDSnowydayBundle:Customer");
        $customers = $repository->findAll();
        return $customers;
    }
 }
 
On rajoute une nouvelle route dans le fichier routing.yml ::

 type: rest
 resource: SD\SnowydayBundle\Controller\CustomersController

Et on peut tester cette nouvelle route avec ::

 curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/customers





curl -X POST -H "Content-Type: application/json" -d '{"username": "yasmany","email": "yasmanycm@gmail.com","password": "ok"} https://snowyday-man.c9users.io/web/app_dev.php/customers > test.html


{"username": "yasmany","email": "yasmanycm@gmail.com","password": "ok"}

 

