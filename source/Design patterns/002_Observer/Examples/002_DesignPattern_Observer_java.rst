DesignPattern Observer java
###########################


.. _observer_Java:

Exemple java
------------
 
::

 // Observer pattern -- Structural example
 // @since JDK 5.0
 import java.util.ArrayList;
 
 // "Subject"
 abstract class Subject {
    // Fields
    private ArrayList<Observer> observers = new ArrayList<Observer>();
    // Methods
    public void attach(Observer observer) {
       observers.add(observer);
    }
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    public void notifyObservers() {
        for (Observer o : observers)    
            o.update();
    } 
 }
 
 // "ConcreteSubject"
 class ConcreteSubject extends Subject {
    // Fields
    private String subjectState;
    // Properties
    public String getSubjectState() {
       return subjectState;
    }
    public void setSubjectState(String value) {
        subjectState = value;
    }
 }

 // "Observer"
 abstract class Observer {
     // Methods
     abstract public void update();
 }


 // "ConcreteObserver"
 class ConcreteObserver extends Observer {
    // Fields
    private String name;
    private String observerState;
    private ConcreteSubject subject;
  
    // Constructors
    public ConcreteObserver(ConcreteSubject subject, String name) {
      this.subject = subject;
      this.name = name;
      //subject.attach(this);
    }
    // Methods
    public void update() {
      observerState = subject.getSubjectState();
      System.out.printf("Observer %s's new state is %s\n", name, observerState);
    }
 }


 // Client test
 public class Client {
    public static void main(String[] args) {
       // Configure Observer structure
       ConcreteSubject s = new ConcreteSubject();
       s.attach(new ConcreteObserver(s, "A"));
        s.attach(new ConcreteObserver(s, "B"));
        s.attach(new ConcreteObserver(s, "C"));
  
       // Change subject and notify observers
       s.setSubjectState("NEW");
       s.notifyObservers();
    }
 }

