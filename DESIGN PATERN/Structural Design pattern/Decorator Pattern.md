
Problem It Solves

It solves the problem of class explosion that occurs when you try to use inheritance to add combinations of behavior. For Example, imagine you have:
A Pizza
A CheesePizza
A CheeseAndOlivePizza
A CheeseAndOliveStuffedPizza

Every combination would need a new subclass as shown in the code below.
Java


```java
import java.util.*;

// Each combination of pizza requires a new class
class PlainPizza {}
class CheesePizza extends PlainPizza {}
class OlivePizza extends PlainPizza {}
class StuffedPizza extends PlainPizza {}
class CheeseStuffedPizza extends CheesePizza {}
class CheeseOlivePizza extends CheesePizza {}
class CheeseOliveStuffedPizza extends CheeseOlivePizza {}

public class Main {
    public static void main(String[] args) {
        // Base pizza
        PlainPizza plainPizza = new PlainPizza();

        // Pizzas with individual toppings
        CheesePizza cheesePizza = new CheesePizza();
        OlivePizza olivePizza = new OlivePizza();
        StuffedPizza stuffedPizza = new StuffedPizza();

        // Combinations of toppings require separate classes
        CheeseStuffedPizza cheeseStuffedPizza = new CheeseStuffedPizza();
        CheeseOlivePizza cheeseOlivePizza = new CheeseOlivePizza();

        // Further combinations increase complexity exponentially
        CheeseOliveStuffedPizza cheeseOliveStuffedPizza = new CheeseOliveStuffedPizza();

    }
}
```

This quickly becomes unmanageable. Here, the Decorator Pattern comes into play. It lets you compose behaviors using wrappers instead of subclassing.

Solution to Pizza Problem

The Decorator Pattern solves the above discussed Pizza problem. It allows us to add responsibilities (like toppings) to objects dynamically without modifying their structure.

Instead of relying on a rigid class hierarchy, we compose objects using wrappers. This promotes flexibility, scalability, and cleaner code architecture.
Using Decorator Pattern


```java
import java.util.*;


interface Pizza{
    String getDescription();
    double getAmount();
}

class MargeritaPizza implements Pizza{

    @Override
    public String getDescription(){
        return "Margerita Pizza";
    }

    @Override
    public double getAmount(){
        return 200;
    }
}

class PlainPizza implements Pizza{

    @Override
    public String getDescription(){
        return "Margerita Pizza";
    }

    @Override
    public double getAmount(){
        return 200;
    }
}

abstract class PizzaDecorator implements Pizza{

    protected Pizza pizza;

    PizzaDecorator(Pizza pizza){
        this.pizza = pizza;
    }
}

class OnionTopping extends PizzaDecorator{

    public OnionTopping(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getDescription(){
        return pizza.getDescription() + " + Onion Topping";
    }

    @Override
    public double getAmount(){
        return pizza.getAmount() + 40;
    }
}

class OlivesTopping extends PizzaDecorator{

    public OlivesTopping(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getDescription(){
        return pizza.getDescription() + " + Olives Topping";
    }

    @Override
    public double getAmount(){
        return pizza.getAmount() + 50;
    }
}

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        Pizza pizza = new OnionTopping(new MargeritaPizza());
        Pizza olivePizza = new OlivesTopping(pizza);

        System.out.println(olivePizza.getDescription());
        sc.close();
    }
}
```