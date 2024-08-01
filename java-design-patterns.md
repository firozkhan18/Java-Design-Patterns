Here’s a comprehensive list of Java design patterns categorized into Creational, Structural, and Behavioral types, along with their uses and brief descriptions:

Java design patterns are typical solutions to common problems in software design, and they are categorized into three main types: Creational, Structural, and Behavioral patterns. Here’s a rundown of the most commonly used Java design patterns, including their use and examples:


### 1. **Creational Design Patterns**

Creational patterns deal with object creation mechanisms, aiming to create objects in a manner suitable to the situation.

#### **1.1 Singleton**
   - **Use**: Ensures a class has only one instance and provides a global point of access to it.
   - **Example**: Database connection manager.

- **Example**:
  ```java
  public class Singleton {
      private static Singleton instance;

      private Singleton() { }

      public static synchronized Singleton getInstance() {
          if (instance == null) {
              instance = new Singleton();
          }
          return instance;
      }
  }
  ```
#### **1.2 Factory Method**
   - **Use**: Defines an interface for creating an object, but allows subclasses to alter the type of objects that will be created.
   - **Use**: Defines an interface for creating an object but lets subclasses alter the type of objects that will be created.
   - **Example**: GUI libraries with different button styles for different platforms.

- **Example**:
  ```java
  interface Product {
      void use();
  }

  class ConcreteProductA implements Product {
      public void use() { System.out.println("Using Product A"); }
  }

  class ConcreteProductB implements Product {
      public void use() { System.out.println("Using Product B"); }
  }

  abstract class Creator {
      public abstract Product factoryMethod();
  }

  class ConcreteCreatorA extends Creator {
      public Product factoryMethod() {
          return new ConcreteProductA();
      }
  }

  class ConcreteCreatorB extends Creator {
      public Product factoryMethod() {
          return new ConcreteProductB();
      }
  }
  ```
#### **1.3 Abstract Factory**
   - **Use**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
   - **Example**: Creating user interfaces in a way that supports multiple themes.

- **Example**:
  ```java
  interface AbstractFactory {
      Product createProductA();
      Product createProductB();
  }

  class ConcreteFactory1 implements AbstractFactory {
      public Product createProductA() {
          return new ProductA1();
      }
      public Product createProductB() {
          return new ProductB1();
      }
  }

  class ConcreteFactory2 implements AbstractFactory {
      public Product createProductA() {
          return new ProductA2();
      }
      public Product createProductB() {
          return new ProductB2();
      }
  }
  ```
  
#### **1.4 Builder**
   - **Use**: Separates the construction of a complex object from its representation so that the same construction process can create different representations.
   - **Example**: Building a complex document with various parts (e.g., HTML, XML).

- **Example**:
  ```java
  class Product {
      private String partA;
      private String partB;
      private String partC;

      public void setPartA(String partA) { this.partA = partA; }
      public void setPartB(String partB) { this.partB = partB; }
      public void setPartC(String partC) { this.partC = partC; }

      @Override
      public String toString() {
          return "Product [partA=" + partA + ", partB=" + partB + ", partC=" + partC + "]";
      }
  }

  interface Builder {
      void buildPartA();
      void buildPartB();
      void buildPartC();
      Product getResult();
  }

  class ConcreteBuilder implements Builder {
      private Product product = new Product();

      public void buildPartA() { product.setPartA("PartA"); }
      public void buildPartB() { product.setPartB("PartB"); }
      public void buildPartC() { product.setPartC("PartC"); }
      public Product getResult() { return product; }
  }
  ```

#### **1.5 Prototype**
   - **Use**: Creates new objects by copying an existing object, known as the prototype.
   - **Example**: Cloning complex objects that are expensive to create from scratch.

- **Example**:
  ```java
  interface Prototype extends Cloneable {
      Prototype clone();
  }

  class ConcretePrototype implements Prototype {
      private String data;

      public ConcretePrototype(String data) { this.data = data; }
      public Prototype clone() {
          return new ConcretePrototype(this.data);
      }
      public String getData() { return data; }
  }
  ```

### 3. **Behavioral Design Patterns**

Behavioral patterns deal with how objects interact and communicate.
Behavioral patterns focus on communication between objects, defining how objects interact and collaborate.

#### **3.1 Chain of Responsibility**
   - **Use**: Passes a request along a chain of handlers, allowing multiple objects to handle the request without the sender needing to know which object will handle it.
   - **Example**: Event handling in GUI systems.

- **Use**: Passes a request along a chain of handlers.
- **Example**:
  ```java
  abstract class Handler {
      private Handler next;

      public void setNext(Handler next) { this.next = next; }
      public void handleRequest(String request) {
          if (next != null) {
              next.handleRequest
2. **Command**
   - **Use**: Encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations.
   - **Example**: Undo/Redo operations in text editors.

3. **Interpreter**
   - **Use**: Defines a grammatical representation for a language and provides an interpreter to interpret sentences in the language.
   - **Example**: Parsing and interpreting expressions or queries.

4. **Iterator**
   - **Use**: Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
   - **Example**: Iterating through elements in a collection (e.g., lists, sets).

5. **Mediator**
   - **Use**: Defines an object that encapsulates how a set of objects interact, promoting loose coupling by preventing objects from referring to each other explicitly.
   - **Example**: Chat room where users communicate through a mediator.

6. **Memento**
   - **Use**: Captures and externalizes an object’s internal state without violating encapsulation, allowing the object to be restored to that state later.
   - **Example**: Saving and restoring game states.

7. **Observer**
   - **Use**: Defines a dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
   - **Example**: Implementing a publish/subscribe system.

8. **State**
   - **Use**: Allows an object to alter its behavior when its internal state changes, appearing as if the object changed its class.
   - **Example**: State-based UI components that change their appearance based on their state.

9. **Strategy**
   - **Use**: Defines a family of algorithms, encapsulates each algorithm, and makes them interchangeable. The strategy pattern lets the algorithm vary independently from clients that use it.
   - **Example**: Sorting algorithms (e.g., quicksort, mergesort) used interchangeably in an application.

10. **Template Method**
    - **Use**: Defines the skeleton of an algorithm in a base class but lets subclasses override specific steps of the algorithm without changing its structure.
    - **Example**: Frameworks that allow for customizations by extending base classes.

11. **Visitor**
    - **Use**: Defines a new operation to a set of objects without changing the classes of the objects.
    - **Example**: Implementing operations on elements of an object structure without altering the elements.

### 2. **Structural Design Patterns**

Structural patterns focus on how classes and objects are composed to form larger structures.

#### **2.1 **Adapter (or Wrapper)**
   - **Use**: Converts the interface of a class into another interface clients expect. It allows classes to work together that couldn’t otherwise due to incompatible interfaces.
   - **Example**: Adapting legacy code to work with a new system.

- **Example**:
  ```java
  interface Target {
      void request();
  }

  class Adaptee {
      void specificRequest() { System.out.println("Specific request"); }
  }

  class Adapter implements Target {
      private Adaptee adaptee;

      public Adapter(Adaptee adaptee) { this.adaptee = adaptee; }
      public void request() { adaptee.specificRequest(); }
  }
  ```
  
#### **2.2 Bridge**
   - **Use**: Decouples an abstraction from its implementation so that the two can vary independently.
   - **Example**: Separating GUI code from the implementation of various platforms.

- **Example**:
  ```java
  interface Implementor {
      void operationImpl();
  }

  class ConcreteImplementorA implements Implementor {
      public void operationImpl() { System.out.println("ConcreteImplementorA operation"); }
  }

  class ConcreteImplementorB implements Implementor {
      public void operationImpl() { System.out.println("ConcreteImplementorB operation"); }
  }

  abstract class Abstraction {
      protected Implementor implementor;
      public Abstraction(Implementor implementor) { this.implementor = implementor; }
      public abstract void operation();
  }

  class RefinedAbstraction extends Abstraction {
      public RefinedAbstraction(Implementor implementor) { super(implementor); }
      public void operation() { implementor.operationImpl(); }
  }
  ```
#### **2.3 Composite**
   - **Use**: Composes objects into tree structures to represent part-whole hierarchies. It allows clients to treat individual objects and compositions of objects uniformly.
   - **Example**: Implementing hierarchical file systems.

- **Example**:
  ```java
  interface Component {
      void operation();
  }

  class Leaf implements Component {
      public void operation() { System.out.println("Leaf operation"); }
  }

  class Composite implements Component {
      private List<Component> children = new ArrayList<>();
      public void add(Component component) { children.add(component); }
      public void remove(Component component) { children.remove(component); }
      public void operation() {
          System.out.println("Composite operation");
          for (Component child : children) {
              child.operation();
          }
      }
  }
  ```
4. **Decorator**
   - **Use**: Adds behavior to objects dynamically without affecting the behavior of other objects from the same class.
   - **Example**: Adding scrolling and borders to a graphical component at runtime.

#### **2.4 Decorator**
- **Use**: Adds behavior to objects dynamically.
- **Example**:
  ```java
  interface Component {
      void operation();
  }

  class ConcreteComponent implements Component {
      public void operation() { System.out.println("ConcreteComponent operation"); }
  }

  abstract class Decorator implements Component {
      protected Component component;
      public Decorator(Component component) { this.component = component; }
      public void operation() { component.operation(); }
  }

  class ConcreteDecoratorA extends Decorator {
      public ConcreteDecoratorA(Component component) { super(component); }
      public void operation() {
          super.operation();
          System.out.println("ConcreteDecoratorA additional operation");
      }
  }
  ```
  
5. **Facade**
   - **Use**: Provides a unified interface to a set of interfaces in a subsystem, making it easier to use.
   - **Example**: Simplified access to a complex library (e.g., simplifying API calls).

#### **2.5 Facade**
- **Use**: Provides a unified interface to a set of interfaces in a subsystem.
- **Example**:
  ```java
  class SubsystemA {
      public void operationA() { System.out.println("SubsystemA operation"); }
  }

  class SubsystemB {
      public void operationB() { System.out.println("SubsystemB operation"); }
  }

  class Facade {
      private SubsystemA a = new SubsystemA();
      private SubsystemB b = new SubsystemB();
      public void unifiedOperation() {
          a.operationA();
          b.operationB();
      }
  }
  ```
6. **Flyweight**
   - **Use**: Reduces the cost of creating and manipulating a large number of similar objects by sharing objects.
   - **Example**: Efficiently managing large numbers of small objects like graphical shapes.

#### **2.6 Flyweight**
- **Use**: Reduces the cost of creating and manipulating a large number of similar objects.
- **Example**:
  ```java
  interface Flyweight {
      void operation();
  }

  class ConcreteFlyweight implements Flyweight {
      private String intrinsicState;

      public ConcreteFlyweight(String intrinsicState) { this.intrinsicState = intrinsicState; }
      public void operation() { System.out.println("ConcreteFlyweight operation with state " + intrinsicState); }
  }

  class FlyweightFactory {
      private Map<String, Flyweight> flyweights = new HashMap<>();

      public Flyweight getFlyweight(String key) {
          if (!flyweights.containsKey(key)) {
              flyweights.put(key, new ConcreteFlyweight(key));
          }
          return flyweights.get(key);
      }
  }
  ```
  
7. **Proxy**
   - **Use**: Provides a surrogate or placeholder for another object to control access to it.
   - **Example**: Implementing lazy initialization, access control, or logging.

#### **2.7 Proxy**
- **Use**: Provides a surrogate or placeholder for another object to control access to it.
- **Example**:
  ```java
  interface Subject {
      void request();
  }

  class RealSubject implements Subject {
      public void request() { System.out.println("RealSubject request"); }
  }

  class Proxy implements Subject {
      private RealSubject realSubject;

      public void request() {
          if (realSubject == null) {
              realSubject = new RealSubject();
          }
          realSubject.request();
      }
  }
  ```
These patterns address various issues and scenarios in object-oriented design, making systems more modular, reusable, and maintainable.

























Creational design patterns focus on object creation mechanisms, aiming to create objects in a manner that is suitable to the situation. They abstract the instantiation process and can handle object creation in various ways, allowing for more flexibility and reusability. Here’s a detailed look at the common creational design patterns in Java, including their uses and examples:

### 1. **Singleton Pattern**

#### **Use**:
- Ensures that a class has only one instance and provides a global point of access to that instance.
- Useful when exactly one object is needed to coordinate actions across the system.

#### **Example**:
```java
public class Singleton {
    // The unique instance of the class
    private static Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() { }

    // Public method to provide access to the instance
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 2. **Factory Method Pattern**

#### **Use**:
- Defines an interface for creating an object but allows subclasses to alter the type of objects that will be created.
- Useful when a class cannot anticipate the class of objects it must create or when subclasses need to specify the objects they create.

#### **Example**:
```java
// Product interface
interface Product {
    void use();
}

// ConcreteProductA
class ConcreteProductA implements Product {
    public void use() {
        System.out.println("Using Product A");
    }
}

// ConcreteProductB
class ConcreteProductB implements Product {
    public void use() {
        System.out.println("Using Product B");
    }
}

// Creator
abstract class Creator {
    public abstract Product factoryMethod();
}

// ConcreteCreatorA
class ConcreteCreatorA extends Creator {
    public Product factoryMethod() {
        return new ConcreteProductA();
    }
}

// ConcreteCreatorB
class ConcreteCreatorB extends Creator {
    public Product factoryMethod() {
        return new ConcreteProductB();
    }
}
```

### 3. **Abstract Factory Pattern**

#### **Use**:
- Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
- Useful when the system needs to be independent of how its products are created, composed, and represented, or when the system needs to be configured with multiple families of products.

#### **Example**:
```java
// Abstract products
interface ProductA {
    void use();
}

interface ProductB {
    void use();
}

// Concrete products
class ConcreteProductA1 implements ProductA {
    public void use() {
        System.out.println("Using Product A1");
    }
}

class ConcreteProductB1 implements ProductB {
    public void use() {
        System.out.println("Using Product B1");
    }
}

class ConcreteProductA2 implements ProductA {
    public void use() {
        System.out.println("Using Product A2");
    }
}

class ConcreteProductB2 implements ProductB {
    public void use() {
        System.out.println("Using Product B2");
    }
}

// Abstract factory
interface AbstractFactory {
    ProductA createProductA();
    ProductB createProductB();
}

// Concrete factories
class ConcreteFactory1 implements AbstractFactory {
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }
    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}

class ConcreteFactory2 implements AbstractFactory {
    public ProductA createProductA() {
        return new ConcreteProductA2();
    }
    public ProductB createProductB() {
        return new ConcreteProductB2();
    }
}
```

### 4. **Builder Pattern**

#### **Use**:
- Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.
- Useful when the construction process must allow different representations for the object that is constructed.

#### **Example**:
```java
// Product
class Product {
    private String partA;
    private String partB;
    private String partC;

    public void setPartA(String partA) { this.partA = partA; }
    public void setPartB(String partB) { this.partB = partB; }
    public void setPartC(String partC) { this.partC = partC; }

    @Override
    public String toString() {
        return "Product [partA=" + partA + ", partB=" + partB + ", partC=" + partC + "]";
    }
}

// Builder interface
interface Builder {
    void buildPartA();
    void buildPartB();
    void buildPartC();
    Product getResult();
}

// ConcreteBuilder
class ConcreteBuilder implements Builder {
    private Product product = new Product();

    public void buildPartA() { product.setPartA("PartA"); }
    public void buildPartB() { product.setPartB("PartB"); }
    public void buildPartC() { product.setPartC("PartC"); }
    public Product getResult() { return product; }
}

// Director
class Director {
    private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }

    public void construct() {
        builder.buildPartA();
        builder.buildPartB();
        builder.buildPartC();
    }
}

// Client code
public class BuilderPatternExample {
    public static void main(String[] args) {
        Builder builder = new ConcreteBuilder();
        Director director = new Director(builder);
        director.construct();
        Product product = builder.getResult();
        System.out.println(product);
    }
}
```

### 5. **Prototype Pattern**

#### **Use**:
- Creates new objects by copying an existing object, known as the prototype.
- Useful when the cost of creating a new instance of an object is more expensive than copying an existing instance.

#### **Example**:
```java
// Prototype interface
interface Prototype extends Cloneable {
    Prototype clone();
}

// ConcretePrototype
class ConcretePrototype implements Prototype {
    private String data;

    public ConcretePrototype(String data) {
        this.data = data;
    }

    public Prototype clone() {
        return new ConcretePrototype(this.data);
    }

    public String getData() {
        return data;
    }
}

// Client code
public class PrototypePatternExample {
    public static void main(String[] args) {
        ConcretePrototype prototype = new ConcretePrototype("Prototype Data");
        ConcretePrototype clone = (ConcretePrototype) prototype.clone();
        System.out.println("Original Data: " + prototype.getData());
        System.out.println("Cloned Data: " + clone.getData());
    }
}
```

Each of these creational design patterns provides a unique way of handling object creation, addressing different needs and scenarios in object-oriented design.
