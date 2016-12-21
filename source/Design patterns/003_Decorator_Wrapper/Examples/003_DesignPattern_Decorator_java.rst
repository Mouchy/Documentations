DesignPattern Decorator java
############################


.. _Decorator_Java:

Exemple java
------------
 
::

 public abstract class Beverage {
    String description = "Unknown Beverage";
  
    public String getDescription() {
        return description;
    }
 
    public abstract double cost();
 }

 public abstract class CondimentDecorator extends Beverage {
    public abstract String getDescription();
 }
 
 public class Mocha extends CondimentDecorator {
    Beverage beverage;
 
    public Mocha(Beverage beverage) {
        this.beverage = beverage;
    }
 
    public String getDescription() {
        return beverage.getDescription() + ", Mocha";
    }
 
    public double cost() {
        return .20 + beverage.cost();
    }
 }

 public class DarkRoast extends Beverage {
    public DarkRoast() {
        description = "Dark Roast Coffee";
    }
 
    public double cost() {
        return .99;
    }
 }

 public class StarbuzzCoffee {
    public static void main(String args[]) {
        Beverage beverage2 = new DarkRoast();
        beverage2 = new Mocha(beverage2);
        beverage2 = new Mocha(beverage2);
        beverage2 = new Whip(beverage2);
        System.out.println(beverage2.getDescription() 
                + " $" + beverage2.cost());
 
    }
 }