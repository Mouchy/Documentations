DesignPattern_Strategy_java
###########################

.. _strategy_Java:

Exemple java
------------

::

 /* IStrategy */
 public interface ComportementVol {
    public void voler();
 }
 
 public class VolerAvecDesAiles implements ComportementVol {
    public void voler(){
        System.out.print("Je vole!!");
    }

 }
 
 public abstract class Canard {
    ComportementVol comportementvol;

    public abstract void afficher();
    
    public void effectuerVol() {
        comportementvol.voler();
    }
    
    public void setComportementVol(ComportementVol cv) {
        comportementvol = cv;
    }
 }

 public class Colvert extends Canard {
    public Colvert() {
        comportementvol = new VolerAvecDesAiles();
    }
    
    public void afficher() {
        System.out.println("Je suis un prototype de canard");
    }
 }

 public class Simulateur {
    public static void main(String[] args){
        Canard colvert = new Colvert();
        colvert.effectuerVol();     
    }
 }