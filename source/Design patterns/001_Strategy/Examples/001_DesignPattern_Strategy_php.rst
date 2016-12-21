Design pattern strategy php
###########################

.. _strategy_PHP:

Exemple PHP
------------

::

 namespace Strategy\Model;

 use Strategy\Model\StrategyInterface;

 class context 
 {
    private $strategy;

     public function __construct(StrategyInterface $strategy) {
        $this->strategy = $strategy;
    }
 
    public function execute() {
        $this->strategy->BehaviourInterface();
    }

 }

 namespace Strategy\Model;

 interface StrategyInterface
 {
    public function BehaviourInterface();
 }

  <?php
 namespace Strategy\Model;

 use Strategy\Model\StrategyInterface;

 Class ConcreteStrategyA implements StrategyInterface
 {
    public function BehaviourInterface()
    {
        echo "Called ConcreteStrategyA execute method\n";
    }
 }
 
 <?php
 namespace Strategy\Model;

 use Strategy\Model\StrategyInterface;

 Class ConcreteStrategyB implements StrategyInterface
 {
    public function BehaviourInterface()
    {
        echo "Called ConcreteStrategyB execute method\n";
    }
 }


 <?php
 namespace Strategy\Controller;

 use Zend\Mvc\Controller\AbstractActionController;
 use Zend\View\Model\ViewModel;
 use Strategy\Model\Context;

 class StrategyController extends AbstractActionController
 {
     protected $context;
     
     public function indexAction()
     {  
        $contextarray = array(
             'context1' => $this->getContext(),
             'context2' => $this->getContext(),
             );
             

         return new ViewModel(array('contexts' =>$contextarray,));
     }

     public function getContext()
     {
         if (!$this->context) {
             $sm = $this->getServiceLocator();
             $this->context = $sm->get('Strategy\Model\Context');
         }
         return $this->context;
     }
 }

 public function getServiceConfig()
 {
 return array(
     'factories' => array(
                         'Strategy\Model\Context' =>  function($sm) {
                             $concretestrategya = new ConcreteStrategyA();
                             $context = new context($concretestrategya);
                             return $context;
                         },
                     ),
              );
 }
 
 
 <?php
 // module/Album/view/album/album/index.phtml:

 $title = 'My albums';
 $this->headTitle($title);
 ?>
 <h1><?php echo $this->escapeHtml($title); ?></h1>
 <?php foreach ($contexts as $context) : ?>

 <tr>
     <td><?php $this->escapeHtml($context->execute());?></td>
 </tr>
 <?php endforeach; ?>
 
 </table>