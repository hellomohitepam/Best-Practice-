## Interface Segregation Principle (ISP)

**Interface Segregation Principle states that clients should not be forced to depend on interfaces they do not use.**

This principle encourages designing **small, specific, and fine-grained interfaces** instead of large, monolithic interfaces.  
By doing so, we avoid forcing classes to implement methods that are irrelevant to them.

---

## ISP Violation Example
```java


// Single interface for all shapes (Violates ISP)
interface Shape {
    double area();
    double volume(); // 2D shapes don't have volume!
}

// Square is a 2D shape but is forced to implement volume()
class Square implements Shape {
    private double side;

    public Square(double s) {
        this.side = s;
    }

    @Override
    public double area() {
        return side * side;
    }

    @Override
    public double volume() {
        throw new UnsupportedOperationException("Volume not applicable for Square"); // Unnecessary method
    }
}

// Rectangle is also a 2D shape but is forced to implement volume()
class Rectangle implements Shape {
    private double length, width;

    public Rectangle(double l, double w) {
        this.length = l;
        this.width  = w;
    }

    @Override
    public double area() {
        return length * width;
    }

    @Override
    public double volume() {
        throw new UnsupportedOperationException("Volume not applicable for Rectangle"); // Unnecessary method
    }
}

// Cube is a 3D shape, so it actually has a volume
class Cube implements Shape {
    private double side;

    public Cube(double s) {
        this.side = s;
    }

    @Override
    public double area() {
        return 6 * side * side;
    }

    @Override
    public double volume() {
        return side * side * side;
    }
}

public class ISPViolated {
    public static void main(String[] args) {
        Shape square    = new Square(5);
        Shape rectangle = new Rectangle(4, 6);
        Shape cube      = new Cube(3);

        System.out.println("Square Area: "    + square.area());
        System.out.println("Rectangle Area: " + rectangle.area());
        System.out.println("Cube Area: "      + cube.area());
        System.out.println("Cube Volume: "    + cube.volume());

        try {
            System.out.println("Square Volume: " + square.volume()); // Will throw an exception
        } catch (UnsupportedOperationException e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }
}

```
in the above code the clasess which are implementing interface are forced to override the volume method which are irrelevent for the square & reactangle
so we create separate the interface 
```java

// Separate interface for 2D shapes
interface TwoDimensionalShape {
    double area();
}

// Separate interface for 3D shapes
interface ThreeDimensionalShape {
    double area();
    double volume();
}

// Square implements only the 2D interface
class Square implements TwoDimensionalShape {
    private double side;

    //constructor
    //area
}

// Rectangle implements only the 2D interface
class Rectangle implements TwoDimensionalShape {
    private double length, width;

    //constructor
    //area
}

// Cube implements the 3D interface
class Cube implements ThreeDimensionalShape {
    private double side;

    //constructor
    //area
    //volume
}

public class ISPFollowed {
    public static void main(String[] args) {
        TwoDimensionalShape square    = new Square(5);
        TwoDimensionalShape rectangle = new Rectangle(4, 6);
        ThreeDimensionalShape cube     = new Cube(3);

        System.out.println("Square Area: "    + square.area());
        System.out.println("Rectangle Area: " + rectangle.area());
        System.out.println("Cube Area: "      + cube.area());
        System.out.println("Cube Volume: "    + cube.volume());
    }
}

```
