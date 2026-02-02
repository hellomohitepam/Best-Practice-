## Decorator

<img width="1162" height="738" alt="image" src="https://github.com/user-attachments/assets/6968c4e5-36f5-46cb-b35a-d7faa48412ff" />


# Decorator Pattern : 
The Decorator Pattern is a structural design pattern that allows you to add new behavior to an object dynamically (at runtime) without modifying its existing code.

# Core Idea:
* Composition over inheritance
* Extend behavior without subclass explosion
* Behaviors can be combined flexibly

`is a` inheritence use to behave like them , `has a` composition relationship for changing behavoiur.

        Component
            ↑
    ConcreteComponent
            ↑
        Decorator
            ↑
     ConcreteDecorator


```java
public interface Coffee {
    double cost();
    String description();
}

public class SimpleCoffee implements Coffee {
    @Override
    public double cost() {
        return 5.0;
    }

    @Override
    public String description() {
        return "Simple Coffee";
    }
}

public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}

public class MilkDecorator extends CoffeeDecorator {

    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double cost() {
        return coffee.cost() + 2.0;
    }

    @Override
    public String description() {
        return coffee.description() + ", Milk";
    }
}

public class SugarDecorator extends CoffeeDecorator {

    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double cost() {
        return coffee.cost() + 1.0;
    }

    @Override
    public String description() {
        return coffee.description() + ", Sugar";
    }
}

public class Main {
    public static void main(String[] args) {

        Coffee coffee = new SimpleCoffee();
        coffee = new MilkDecorator(coffee);
        coffee = new SugarDecorator(coffee);

        System.out.println(coffee.description());
        System.out.println("Cost: " + coffee.cost());
    }
}
```
```
Simple Coffee, Milk, Sugar
Cost: 8.0
```

# Follows SOLID Principles
| Principle | How Decorator Helps                          |
| --------- | -------------------------------------------- |
| OCP       | Add behavior without modifying existing code |
| SRP       | Each decorator has one responsibility        |
| DIP       | Depends on abstraction (`Coffee`)            |


# ✅ Use when:
* You need to add responsibilities dynamically
* Subclassing leads to class explosion
* You want flexible feature combinations

# ❌ Avoid when:
* You need to add behavior to all objects globally
* Excessive decorators make debugging hard

# Advantages
* Open/Closed Principle
* Runtime flexibility
* Cleaner than large inheritance trees
* Behavior reuse

# Disadvantages
* Many small classes
* Harder to debug (nested objects)
* Order of decorators may matter

# Decorator vs Proxy vs Adapter (Quick Comparison)
| Pattern   | Purpose           |
| --------- | ----------------- |
| Decorator | Add behavior      |
| Proxy     | Control access    |
| Adapter   | Convert interface |

