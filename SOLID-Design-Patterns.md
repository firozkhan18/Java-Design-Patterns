graph TD
    A[Client] --> B[API Gateway]
    B --> C[Service A]
    B --> D[Service B]
    B --> E[Service C]

    B --> |Routing| F[API Gateway Details]
    B --> |Load Balancing| F
    B --> |Authentication| F
    B --> |Aggregation| F

    C --> G[Database A]
    D --> H[Database B]
    E --> I[Database C]

    subgraph API_Gateway_Details
        F
    end

    subgraph Microservices
        C
        D
        E
    end

    subgraph Databases
        G
        H
        I
    end

    classDef client fill:#f9f,stroke:#333,stroke-width:2px;
    classDef apiGateway fill:#ccf,stroke:#333,stroke-width:2px;
    classDef microservice fill:#cfc,stroke:#333,stroke-width:2px;
    classDef database fill:#fcf,stroke:#333,stroke-width:2px;

    class A client;
    class B apiGateway;
    class C,D,E microservice;
    class G,H,I database;


Certainly! The SOLID principles are a set of five design principles that help developers create software that is easy to manage and scale. Here’s an in-depth explanation of each SOLID principle with Java code examples.

### 1. Single Responsibility Principle (SRP)

**Principle**: A class should have only one reason to change, meaning it should only have one job or responsibility.

**Violation Example**:
```java
public class Employee {
    private String name;
    private double salary;

    public void calculateSalary() {
        // Calculate salary logic
    }

    public void generatePayrollReport() {
        // Generate payroll report logic
    }
}
```
In the above example, the `Employee` class has two responsibilities: managing employee data and generating payroll reports. This violates SRP.

**Adhering to SRP**:
```java
public class Employee {
    private String name;
    private double salary;

    public void calculateSalary() {
        // Calculate salary logic
    }
}

public class PayrollReportGenerator {
    public void generatePayrollReport(Employee employee) {
        // Generate payroll report logic
    }
}
```
Here, the `Employee` class is only responsible for salary calculations, while the `PayrollReportGenerator` handles report generation, adhering to SRP.

### 2. Open/Closed Principle (OCP)

**Principle**: Software entities should be open for extension but closed for modification. This means you should be able to add new functionality without changing existing code.

**Violation Example**:
```java
public class Shape {
    public double calculateArea() {
        // Default implementation
        return 0;
    }
}

public class AreaCalculator {
    public double calculateTotalArea(Shape[] shapes) {
        double total = 0;
        for (Shape shape : shapes) {
            if (shape instanceof Circle) {
                // Specific handling for Circle
                Circle circle = (Circle) shape;
                total += Math.PI * circle.getRadius() * circle.getRadius();
            } else if (shape instanceof Rectangle) {
                // Specific handling for Rectangle
                Rectangle rectangle = (Rectangle) shape;
                total += rectangle.getWidth() * rectangle.getHeight();
            }
        }
        return total;
    }
}
```
In this example, if a new shape is introduced, the `AreaCalculator` class must be modified, violating OCP.

**Adhering to OCP**:
```java
public abstract class Shape {
    public abstract double calculateArea();
}

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return width * height;
    }
}

public class AreaCalculator {
    public double calculateTotalArea(Shape[] shapes) {
        double total = 0;
        for (Shape shape : shapes) {
            total += shape.calculateArea();
        }
        return total;
    }
}
```
Here, `Shape` is an abstract class with an abstract method `calculateArea()`. New shapes can be added without modifying `AreaCalculator`, adhering to OCP.

### 3. Liskov Substitution Principle (LSP)

**Principle**: Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.

**Violation Example**:
```java
public class Bird {
    public void fly() {
        // Flying logic
    }
}

public class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly");
    }
}
```
In this example, `Penguin` is a subclass of `Bird`, but it cannot fly, which violates LSP because substituting a `Bird` with a `Penguin` leads to incorrect behavior.

**Adhering to LSP**:
```java
public abstract class Bird {
    // Common bird behaviors
}

public interface Flyable {
    void fly();
}

public class Sparrow extends Bird implements Flyable {
    @Override
    public void fly() {
        // Flying logic
    }
}

public class Penguin extends Bird {
    // No fly method
}
```
Here, `Flyable` is an interface for flying behavior, allowing `Sparrow` to implement it without forcing `Penguin` to have a `fly` method. This respects LSP.

### 4. Interface Segregation Principle (ISP)

**Principle**: Clients should not be forced to depend on interfaces they do not use. Interfaces should be specific to the client’s needs.

**Violation Example**:
```java
public interface Worker {
    void work();
    void eat();
}

public class HumanWorker implements Worker {
    @Override
    public void work() {
        // Work logic
    }

    @Override
    public void eat() {
        // Eating logic
    }
}

public class RobotWorker implements Worker {
    @Override
    public void work() {
        // Work logic
    }

    @Override
    public void eat() {
        throw new UnsupportedOperationException("Robots don't eat");
    }
}
```
In this example, `RobotWorker` does not need the `eat` method, which violates ISP.

**Adhering to ISP**:
```java
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}

public class HumanWorker implements Workable, Eatable {
    @Override
    public void work() {
        // Work logic
    }

    @Override
    public void eat() {
        // Eating logic
    }
}

public class RobotWorker implements Workable {
    @Override
    public void work() {
        // Work logic
    }
}
```
Here, `Workable` and `Eatable` are separate interfaces. `HumanWorker` implements both, while `RobotWorker` only implements `Workable`, adhering to ISP.

### 5. Dependency Inversion Principle (DIP)

**Principle**: High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

**Violation Example**:
```java
public class LightBulb {
    public void turnOn() {
        // Turn on the light
    }

    public void turnOff() {
        // Turn off the light
    }
}

public class Switch {
    private LightBulb bulb;

    public Switch(LightBulb bulb) {
        this.bulb = bulb;
    }

    public void operate() {
        // Turn on/off the light bulb
    }
}
```
In this example, the `Switch` class directly depends on the `LightBulb` class, which violates DIP.

**Adhering to DIP**:
```java
public interface Switchable {
    void turnOn();
    void turnOff();
}

public class LightBulb implements Switchable {
    @Override
    public void turnOn() {
        // Turn on the light
    }

    @Override
    public void turnOff() {
        // Turn off the light
    }
}

public class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        // Use the device's turnOn/turnOff methods
    }
}
```
Here, `Switch` depends on the `Switchable` interface rather than a concrete class. This allows `Switch` to work with any device that implements `Switchable`, adhering to DIP.

### Summary

- **Single Responsibility Principle (SRP)**: Each class should have only one reason to change.
- **Open/Closed Principle (OCP)**: Entities should be open for extension but closed for modification.
- **Liskov Substitution Principle (LSP)**: Subtypes must be replaceable for their base types without affecting correctness.
- **Interface Segregation Principle (ISP)**: Clients should not be forced to depend on interfaces they do not use.
- **Dependency Inversion Principle (DIP)**: High-level modules should depend on abstractions, not on concrete implementations.

By applying these principles, you create software that is more modular, maintainable, and adaptable to change.
