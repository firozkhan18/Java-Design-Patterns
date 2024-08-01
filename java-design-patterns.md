Java design patterns are typical solutions to common problems in software design, and they are categorized into three main types: Creational, Structural, and Behavioral patterns. Here’s a rundown of the most commonly used Java design patterns, including their use and examples:

### 1. **Creational Patterns**
Creational patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.

#### **1.1 Singleton**
- **Use**: Ensures that a class has only one instance and provides a global point of access to it.
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
- **Use**: Defines an interface for creating an object but lets subclasses alter the type of objects that will be created.
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

### 2. **Structural Patterns**
Structural patterns focus on how classes and objects are composed to form larger structures.

#### **2.1 Adapter**
- **Use**: Converts the interface of a class into another interface that clients expect.
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
- **Use**: Allows you to compose objects into tree structures to represent part-whole hierarchies.
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

### 3. **Behavioral Patterns**
Behavioral patterns deal with how objects interact and communicate.

#### **3.1 Chain of Responsibility**
- **Use**: Passes a request along a chain of handlers.
- **Example**:
  ```java
  abstract class Handler {
      private Handler next;

      public void setNext(Handler next) { this.next = next; }
      public void handleRequest(String request) {
          if (next != null) {
              next.handleRequest


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
