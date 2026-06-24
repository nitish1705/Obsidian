
Basic Structure of Factory Pattern

The Factory Pattern typically consists of the following components:
Product: It is an interface or abstract class that defines the methods the product must implement.
Concrete Products: The concrete classes that implement the Product interface.
Factory: A class with a method that returns different concrete products based on input.

Consider the following example code snippet:
Java


```java
// Interface 
interface Shape {
    void draw();
}

// Class implementing the Shape Interface
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

// Class implementing the Shape Interface
class Square implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Square");
    }
}

// Factory Class
class ShapeFactory {
    /* Method that takes the type of shape as input 
    and returns the cirresponding object */
    public Shape getShape(String shapeType) {
        if (shapeType.equalsIgnoreCase("CIRCLE")) {
            return new Circle();
        } else if (shapeType.equalsIgnoreCase("SQUARE")) {
            return new Square();
        }
        return null;
    }
}

// Driver code
class Main {
    public static void main(String[] args) {
        // Object of ShapeFactory is initialized
        ShapeFactory shapeFactory = new ShapeFactory();

        // Get a Circle object and call its draw method
        Shape shape1 = shapeFactory.getShape("CIRCLE");
        shape1.draw();

        // Get a Square object and call its draw method
        Shape shape2 = shapeFactory.getShape("SQUARE");
        shape2.draw();
    }
}
```

Here, ShapeFactory is the factory that returns different objects (Circle, Square) based on input.

Real-life Product Example - Logistics Services

Let's say you are building a logistics application that needs to handle different types of transport services: By Road, By Air, etc.

Bad Practice: Not Following Factory Pattern

Consider the following code snippet where object creation logic is tightly coupled with business logic:
Java


```java
// Logistics Interface
interface Logistics {
    void send();
}

// Class implementing the Logistics Interface
class Road implements Logistics {
    @Override
    public void send() {
        System.out.println("Sending by road logic");
    }
}

// Class implementing the Logistics Interface
class Air implements Logistics {
    @Override
    public void send() {
        System.out.println("Sending by air logic");
    }
}

// Class implementing Logistics Service
class LogisticsService {
    public void send(String mode) {
        if (mode.equals("Air")) {
            Logistics logistics = new Air();
            logistics.send();
        } else if (mode.equals("Road")) {
            Logistics logistics = new Road();
            logistics.send();
        }
    }
}

// Driver code
class Main {
    public static void main(String[] args) {
        LogisticsService service = new LogisticsService();
        service.send("Air");
        service.send("Road");
    }
}
```
Understanding the Issue:

In the LogisticsService class:
The object of Air or Road is directly instantiated based on string comparison.
The object creation logic is embedded inside the business logic (send method).
This violates the Open/Closed Principle — if you want to add a new mode (e.g., Ship), you have to modify the send method.
Problems:

Tight Coupling: LogisticsService depends directly on Air and Road classes.
Hard to Extend: Adding a new mode (e.g., Drone, Ship) requires modifying existing code.
No Separation of Concerns: Object creation and business logic are mixed.
Code Duplication: Repeated instantiation and send() logic.
Testing & Maintenance Nightmare: Hard to test independently or mock logistics.

Good Practice: Following Factory Pattern

Let's now apply the Factory Pattern to clean this up and make it scalable.
Java

```java
// Logistic Interface
interface Logistics {
    void send();
}

// Class implementing the Logistics Interface
class Road implements Logistics {
    @Override
    public void send() {
        System.out.println("Sending by road logic");
    }
}

// Class implementing the Logistics Interface
class Air implements Logistics {
    @Override
    public void send() {
        System.out.println("Sending by air logic");
    }
}

// Factory Class taking care of Logistics
class LogisticsFactory {
    public static Logistics getLogistics(String mode) {
        if (mode.equalsIgnoreCase("Air")) {
            return new Air();
        } else if (mode.equalsIgnoreCase("Road")) {
            return new Road();
        }
        throw new IllegalArgumentException("Unknown logistics mode: " + mode);
    }
}

// Class implementing the Logistics Services
class LogisticsService {
    public void send(String mode) {
        /* Using the Logistics Factory to get the 
        desired object based on the mode */
        Logistics logistics = LogisticsFactory.getLogistics(mode);
        logistics.send();
    }
}

// Driver Code
class Main {
    public static void main(String[] args) {
        LogisticsService service = new LogisticsService();
        service.send("Air");
        service.send("Road");
    }
}
```
Understanding the Improvement:

In this refactored code:
The object creation logic is moved to the LogisticsFactory.
The LogisticsService class now only focuses on business logic.
Adding a new mode (e.g., Ship) only requires modifying the factory, not the service.
