Certainly! Here’s a detailed look at each design pattern within the Creational, Behavioral, and Structural categories, complete with explanations and code examples:

## **Creational Design Patterns**

### 1. **Singleton**

#### **Use**:
- Ensures that a class has only one instance and provides a global point of access to it.
- Useful for managing shared resources, such as a configuration manager or a connection pool.

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

### 2. **Factory Method**

#### **Use**:
- Defines an interface for creating an object but allows subclasses to alter the type of objects that will be created.
- Useful for managing and maintaining a family of related objects.

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

// Client code
public class FactoryMethodExample {
    public static void main(String[] args) {
        Creator creator = new ConcreteCreatorA();
        Product product = creator.factoryMethod();
        product.use();  // Output: Using Product A
    }
}
```

### 3. **Abstract Factory**

#### **Use**:
- Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
- Useful when you need to create multiple families of products that need to work together.

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

// Client code
public class AbstractFactoryExample {
    public static void main(String[] args) {
        AbstractFactory factory = new ConcreteFactory1();
        ProductA productA = factory.createProductA();
        ProductB productB = factory.createProductB();
        productA.use();  // Output: Using Product A1
        productB.use();  // Output: Using Product B1
    }
}
```

### 4. **Builder**

#### **Use**:
- Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.
- Useful for constructing complex objects with many optional components.

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
public class BuilderExample {
    public static void main(String[] args) {
        Builder builder = new ConcreteBuilder();
        Director director = new Director(builder);
        director.construct();
        Product product = builder.getResult();
        System.out.println(product);
    }
}
```

### 5. **Prototype**

#### **Use**:
- Creates new objects by copying an existing object, known as the prototype.
- Useful when creating new instances of a class is costly and the class can be copied.

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
public class PrototypeExample {
    public static void main(String[] args) {
        ConcretePrototype prototype = new ConcretePrototype("Prototype Data");
        ConcretePrototype clone = (ConcretePrototype) prototype.clone();
        System.out.println("Original Data: " + prototype.getData());  // Output: Prototype Data
        System.out.println("Cloned Data: " + clone.getData());        // Output: Prototype Data
    }
}
```

## **Behavioral Design Patterns**

### 1. **Chain of Responsibility**

#### **Use**:
- Passes a request along a chain of handlers, allowing multiple objects to handle the request without the sender needing to know which object will handle it.
- Useful for event handling systems where multiple handlers might handle a request.

#### **Example**:
```java
// Handler
abstract class Handler {
    private Handler next;

    public void setNext(Handler next) { this.next = next; }
    public void handleRequest(String request) {
        if (next != null) {
            next.handleRequest(request);
        }
    }
}

// ConcreteHandler1
class ConcreteHandler1 extends Handler {
    public void handleRequest(String request) {
        if (request.equals("Handler1")) {
            System.out.println("Handled by ConcreteHandler1");
        } else {
            super.handleRequest(request);
        }
    }
}

// ConcreteHandler2
class ConcreteHandler2 extends Handler {
    public void handleRequest(String request) {
        if (request.equals("Handler2")) {
            System.out.println("Handled by ConcreteHandler2");
        } else {
            super.handleRequest(request);
        }
    }
}

// Client code
public class ChainOfResponsibilityExample {
    public static void main(String[] args) {
        Handler handler1 = new ConcreteHandler1();
        Handler handler2 = new ConcreteHandler2();
        handler1.setNext(handler2);

        handler1.handleRequest("Handler1");  // Output: Handled by ConcreteHandler1
        handler1.handleRequest("Handler2");  // Output: Handled by ConcreteHandler2
        handler1.handleRequest("Unknown");   // No output
    }
}
```

### 2. **Command**

#### **Use**:
- Encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations.
- Useful for implementing undo/redo functionality or managing complex command operations.

#### **Example**:
```java
// Command interface
interface Command {
    void execute();
}

// ConcreteCommand
class ConcreteCommand implements Command {
    private Receiver receiver;

    public ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    public void execute() {
        receiver.action();
    }
}

// Receiver
class Receiver {
    public void action() {
        System.out.println("Receiver action executed");
    }
}

// Invoker
class Invoker {
    private Command command;

    public void setCommand(Command command) { this.command = command; }
    public void invoke() { command.execute(); }
}

// Client code
public class CommandExample {
    public static void main(String[] args) {
        Receiver receiver = new Receiver();
        Command command = new ConcreteCommand(receiver);
        Invoker invoker = new Invoker();
        invoker.setCommand(command);
        invoker.invoke();  // Output: Receiver action executed
    }
}
```

### 3. **Interpreter**

#### **Use**:
- Defines a grammatical representation for a language and provides

 an interpreter to interpret sentences in the language.
- Useful for designing languages or interpreters.

#### **Example**:
```java
// AbstractExpression
interface Expression {
    boolean interpret(String context);
}

// TerminalExpression
class TerminalExpression implements Expression {
    private String data;

    public TerminalExpression(String data) {
        this.data = data;
    }

    public boolean interpret(String context) {
        return context.contains(data);
    }
}

// OrExpression
class OrExpression implements Expression {
    private Expression expr1;
    private Expression expr2;

    public OrExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
}

// Client code
public class InterpreterExample {
    public static void main(String[] args) {
        Expression isJava = new TerminalExpression("Java");
        Expression isJavaScript = new TerminalExpression("JavaScript");
        Expression orExpression = new OrExpression(isJava, isJavaScript);

        System.out.println("Is Java? " + orExpression.interpret("Java"));          // Output: Is Java? true
        System.out.println("Is JavaScript? " + orExpression.interpret("JavaScript")); // Output: Is JavaScript? true
        System.out.println("Is Python? " + orExpression.interpret("Python"));       // Output: Is Python? false
    }
}
```

### 4. **Iterator**

#### **Use**:
- Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
- Useful for iterating over elements in collections.

#### **Example**:
```java
// Iterator interface
interface Iterator {
    boolean hasNext();
    Object next();
}

// Iterable interface
interface IterableCollection {
    Iterator createIterator();
}

// ConcreteIterator
class ConcreteIterator implements Iterator {
    private String[] items;
    private int position;

    public ConcreteIterator(String[] items) {
        this.items = items;
        this.position = 0;
    }

    public boolean hasNext() {
        return position < items.length;
    }

    public Object next() {
        return items[position++];
    }
}

// ConcreteCollection
class ConcreteCollection implements IterableCollection {
    private String[] items;

    public ConcreteCollection(String[] items) {
        this.items = items;
    }

    public Iterator createIterator() {
        return new ConcreteIterator(items);
    }
}

// Client code
public class IteratorExample {
    public static void main(String[] args) {
        String[] items = {"Item1", "Item2", "Item3"};
        IterableCollection collection = new ConcreteCollection(items);
        Iterator iterator = collection.createIterator();

        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

### 5. **Mediator**

#### **Use**:
- Defines an object that encapsulates how a set of objects interact, promoting loose coupling by preventing objects from referring to each other explicitly.
- Useful for managing complex communications between multiple objects.

#### **Example**:
```java
// Mediator interface
interface Mediator {
    void send(String message, Colleague colleague);
}

// Colleague
abstract class Colleague {
    protected Mediator mediator;

    public Colleague(Mediator mediator) {
        this.mediator = mediator;
    }
}

// ConcreteColleague
class ConcreteColleague1 extends Colleague {
    public ConcreteColleague1(Mediator mediator) {
        super(mediator);
    }

    public void send(String message) {
        mediator.send(message, this);
    }

    public void receive(String message) {
        System.out.println("Colleague1 received: " + message);
    }
}

// ConcreteColleague
class ConcreteColleague2 extends Colleague {
    public ConcreteColleague2(Mediator mediator) {
        super(mediator);
    }

    public void receive(String message) {
        System.out.println("Colleague2 received: " + message);
    }
}

// ConcreteMediator
class ConcreteMediator implements Mediator {
    private ConcreteColleague1 colleague1;
    private ConcreteColleague2 colleague2;

    public void setColleague1(ConcreteColleague1 colleague1) {
        this.colleague1 = colleague1;
    }

    public void setColleague2(ConcreteColleague2 colleague2) {
        this.colleague2 = colleague2;
    }

    public void send(String message, Colleague colleague) {
        if (colleague == colleague1) {
            colleague2.receive(message);
        } else {
            colleague1.receive(message);
        }
    }
}

// Client code
public class MediatorExample {
    public static void main(String[] args) {
        ConcreteMediator mediator = new ConcreteMediator();
        ConcreteColleague1 colleague1 = new ConcreteColleague1(mediator);
        ConcreteColleague2 colleague2 = new ConcreteColleague2(mediator);
        mediator.setColleague1(colleague1);
        mediator.setColleague2(colleague2);

        colleague1.send("Hello, Colleague2");
        colleague2.send("Hello, Colleague1");
    }
}
```

### 6. **Memento**

#### **Use**:
- Captures and externalizes an object’s internal state without violating encapsulation, allowing the object to be restored to that state later.
- Useful for undo/redo functionality or saving the state of an object.

#### **Example**:
```java
// Memento
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

// Originator
class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public Memento saveStateToMemento() {
        return new Memento(state);
    }

    public void getStateFromMemento(Memento memento) {
        state = memento.getState();
    }

    @Override
    public String toString() {
        return "Originator{" + "state='" + state + '\'' + '}';
    }
}

// Caretaker
class Caretaker {
    private Memento memento;

    public void saveMemento(Memento memento) {
        this.memento = memento;
    }

    public Memento getMemento() {
        return memento;
    }
}

// Client code
public class MementoExample {
    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        originator.setState("State1");
        caretaker.saveMemento(originator.saveStateToMemento());

        originator.setState("State2");
        System.out.println("Current State: " + originator);

        originator.getStateFromMemento(caretaker.getMemento());
        System.out.println("Restored State: " + originator);
    }
}
```

### 7. **Observer**

#### **Use**:
- Defines a dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
- Useful for implementing a subscription mechanism or notification system.

#### **Example**:
```java
// Subject
interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// Observer
interface Observer {
    void update(String message);
}

// ConcreteSubject
class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String message;

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void setMessage(String message) {
        this.message = message;
        notifyObservers();
    }
}

// ConcreteObserver
class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " received message: " + message);
    }
}

// Client code
public class ObserverExample {
    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();
        Observer observer1 = new ConcreteObserver("Observer1");
        Observer observer2 = new ConcreteObserver("Observer2");

        subject.addObserver(observer1);
        subject.addObserver(observer2);

        subject.setMessage("Hello Observers");
    }
}
```

### 8. **State**

#### **Use**:
- Allows an object to alter its behavior when its internal state changes, appearing as if the object changed its class.
- Useful for managing state-specific behavior in objects.

#### **Example**:
```java
// State interface
interface State {
    void handleRequest();
}

// ConcreteStateA
class ConcreteStateA implements State {
    public void handleRequest() {
        System.out.println("Handling request in State A");
    }
}

// ConcreteStateB
class ConcreteStateB implements State {
    public void handleRequest() {
        System.out.println("Handling request in State B");
    }
}

// Context
class Context {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public void request() {
        state.handleRequest();
    }
}

// Client code
public class StateExample {
    public static void main(String[] args) {
        Context context = new Context();

        State stateA = new ConcreteStateA();
        State stateB = new ConcreteStateB();

        context.setState(stateA);
        context.request();  // Output: Handling request in State A

        context.setState(stateB);
        context.request();  // Output: Handling request in State B
    }
}
```

### 9. **Strategy**

#### **Use

**:
- Defines a family of algorithms, encapsulates each algorithm, and makes them interchangeable. The strategy pattern lets the algorithm vary independently from clients that use it.
- Useful for implementing different algorithms or strategies in a flexible and reusable manner.

#### **Example**:
```java
// Strategy interface
interface Strategy {
    int execute(int a, int b);
}

// ConcreteStrategyA
class ConcreteStrategyA implements Strategy {
    public int execute(int a, int b) {
        return a + b;
    }
}

// ConcreteStrategyB
class ConcreteStrategyB implements Strategy {
    public int execute(int a, int b) {
        return a * b;
    }
}

// Context
class Context {
    private Strategy strategy;

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public int executeStrategy(int a, int b) {
        return strategy.execute(a, b);
    }
}

// Client code
public class StrategyExample {
    public static void main(String[] args) {
        Context context = new Context();

        context.setStrategy(new ConcreteStrategyA());
        System.out.println("Result: " + context.executeStrategy(5, 3));  // Output: Result: 8

        context.setStrategy(new ConcreteStrategyB());
        System.out.println("Result: " + context.executeStrategy(5, 3));  // Output: Result: 15
    }
}
```

### 10. **Template Method**

#### **Use**:
- Defines the skeleton of an algorithm in a base class but lets subclasses override specific steps of the algorithm without changing its structure.
- Useful for defining a template for operations where some steps can be customized.

#### **Example**:
```java
// AbstractClass
abstract class AbstractClass {
    // Template method
    public final void templateMethod() {
        step1();
        step2();
        step3();
    }

    // Concrete methods
    private void step1() {
        System.out.println("Step 1");
    }

    // Abstract methods
    protected abstract void step2();
    protected abstract void step3();
}

// ConcreteClassA
class ConcreteClassA extends AbstractClass {
    protected void step2() {
        System.out.println("ConcreteClassA Step 2");
    }

    protected void step3() {
        System.out.println("ConcreteClassA Step 3");
    }
}

// ConcreteClassB
class ConcreteClassB extends AbstractClass {
    protected void step2() {
        System.out.println("ConcreteClassB Step 2");
    }

    protected void step3() {
        System.out.println("ConcreteClassB Step 3");
    }
}

// Client code
public class TemplateMethodExample {
    public static void main(String[] args) {
        AbstractClass classA = new ConcreteClassA();
        classA.templateMethod();
        // Output:
        // Step 1
        // ConcreteClassA Step 2
        // ConcreteClassA Step 3

        AbstractClass classB = new ConcreteClassB();
        classB.templateMethod();
        // Output:
        // Step 1
        // ConcreteClassB Step 2
        // ConcreteClassB Step 3
    }
}
```

### 11. **Visitor**

#### **Use**:
- Defines a new operation to a set of objects without changing the classes of the objects.
- Useful for operations that need to be performed on elements of an object structure but don’t want to alter the elements themselves.

#### **Example**:
```java
// Visitor interface
interface Visitor {
    void visit(Element element);
}

// ConcreteVisitor
class ConcreteVisitor implements Visitor {
    public void visit(Element element) {
        System.out.println("Visiting " + element);
    }
}

// Element interface
interface Element {
    void accept(Visitor visitor);
}

// ConcreteElement
class ConcreteElement implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }

    @Override
    public String toString() {
        return "ConcreteElement";
    }
}

// Client code
public class VisitorExample {
    public static void main(String[] args) {
        Element element = new ConcreteElement();
        Visitor visitor = new ConcreteVisitor();
        element.accept(visitor);  // Output: Visiting ConcreteElement
    }
}
```

## **Structural Design Patterns**

### 1. **Adapter (or Wrapper)**

#### **Use**:
- Converts the interface of a class into another interface clients expect. It allows classes to work together that couldn’t otherwise due to incompatible interfaces.
- Useful for integrating legacy systems with new systems.

#### **Example**:
```java
// Target interface
interface Target {
    void request();
}

// Adaptee
class Adaptee {
    public void specificRequest() {
        System.out.println("Specific request");
    }
}

// Adapter
class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    public void request() {
        adaptee.specificRequest();
    }
}

// Client code
public class AdapterExample {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);
        target.request();  // Output: Specific request
    }
}
```

### 2. **Bridge**

#### **Use**:
- Decouples an abstraction from its implementation so that the two can vary independently.
- Useful for managing cross-cutting concerns where the abstraction and implementation need to evolve independently.

#### **Example**:
```java
// Abstraction
abstract class Abstraction {
    protected Implementor implementor;

    public Abstraction(Implementor implementor) {
        this.implementor = implementor;
    }

    public abstract void operation();
}

// RefinedAbstraction
class RefinedAbstraction extends Abstraction {
    public RefinedAbstraction(Implementor implementor) {
        super(implementor);
    }

    public void operation() {
        implementor.implementation();
    }
}

// Implementor
interface Implementor {
    void implementation();
}

// ConcreteImplementorA
class ConcreteImplementorA implements Implementor {
    public void implementation() {
        System.out.println("ConcreteImplementorA implementation");
    }
}

// ConcreteImplementorB
class ConcreteImplementorB implements Implementor {
    public void implementation() {
        System.out.println("ConcreteImplementorB implementation");
    }
}

// Client code
public class BridgeExample {
    public static void main(String[] args) {
        Implementor implementorA = new ConcreteImplementorA();
        Abstraction abstractionA = new RefinedAbstraction(implementorA);
        abstractionA.operation();  // Output: ConcreteImplementorA implementation

        Implementor implementorB = new ConcreteImplementorB();
        Abstraction abstractionB = new RefinedAbstraction(implementorB);
        abstractionB.operation();  // Output: ConcreteImplementorB implementation
    }
}
```

### 3. **Composite**

#### **Use**:
- Composes objects into tree structures to represent part-whole hierarchies. It allows clients to treat individual objects and compositions of objects uniformly.
- Useful for implementing tree-like structures such as filesystems or organizational hierarchies.

#### **Example**:
```java
// Component
abstract class Component {
    public abstract void operation();
}

// Leaf
class Leaf extends Component {
    public void operation() {
        System.out.println("Leaf operation");
    }
}

// Composite
class Composite extends Component {
    private List<Component> children = new ArrayList<>();

    public void add(Component component) {
        children.add(component);
    }

    public void remove(Component component) {
        children.remove(component);
    }

    public void operation() {
        System.out.println("Composite operation");
        for (Component child : children) {
            child.operation();
        }
    }
}

// Client code
public class CompositeExample {
    public static void main(String[] args) {
        Component leaf1 = new Leaf();
        Component leaf2 = new Leaf();
        Composite composite = new Composite();
        composite.add(leaf1);
        composite.add(leaf2);
        composite.operation();  
        // Output:
        // Composite operation
        // Leaf operation
        // Leaf operation
    }
}
```

### 4. **Decorator**

#### **Use**:
- Adds behavior to objects dynamically without affecting the behavior of other objects from the same class.
- Useful for extending the functionality of objects in a flexible and reusable manner.

#### **Example**:
```java
// Component
interface Component {
    void operation();
}

// ConcreteComponent
class ConcreteComponent implements Component {
    public void operation() {
        System.out.println("ConcreteComponent operation");
    }
}

// Decorator
abstract class Decorator implements Component {
    protected Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    public void operation() {
        component.operation();
    }
}

// ConcreteDecoratorA
class ConcreteDecoratorA extends Decorator {
    public ConcreteDecoratorA(Component component) {
        super(component);
    }

    public void operation() {
        super.operation();
        System.out.println("ConcreteDecoratorA operation");
    }
}

// ConcreteDecoratorB
class ConcreteDecoratorB extends Decorator {
    public ConcreteDecoratorB(Component component) {
        super(component);
    }

    public void operation() {
        super.operation();
        System.out.println("ConcreteDecoratorB operation");
    }
}

// Client code
public class DecoratorExample {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        Component decoratorA = new ConcreteDecoratorA(component);
        Component decoratorB = new ConcreteDecoratorB(decoratorA);
        decoratorB.operation();
        // Output:
        // ConcreteComponent operation
        // ConcreteDecoratorA operation
        // ConcreteDecoratorB operation
    }
}
```

### 5. **Facade**

#### **Use**:
- Provides a unified interface to a set of interfaces in a subsystem, making it easier to use.


- Useful for simplifying the use of complex systems by providing a higher-level interface.

#### **Example**:
```java
// Subsystem classes
class SubsystemA {
    public void operationA() {
        System.out.println("SubsystemA operation");
    }
}

class SubsystemB {
    public void operationB() {
        System.out.println("SubsystemB operation");
    }
}

class SubsystemC {
    public void operationC() {
        System.out.println("SubsystemC operation");
    }
}

// Facade
class Facade {
    private SubsystemA subsystemA;
    private SubsystemB subsystemB;
    private SubsystemC subsystemC;

    public Facade() {
        subsystemA = new SubsystemA();
        subsystemB = new SubsystemB();
        subsystemC = new SubsystemC();
    }

    public void performOperation() {
        subsystemA.operationA();
        subsystemB.operationB();
        subsystemC.operationC();
    }
}

// Client code
public class FacadeExample {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.performOperation();
        // Output:
        // SubsystemA operation
        // SubsystemB operation
        // SubsystemC operation
    }
}
```

### 6. **Flyweight**

#### **Use**:
- Provides a way to reduce the cost of creating and manipulating a large number of similar objects by sharing common parts of the state.
- Useful for managing a large number of objects efficiently by sharing common data.

#### **Example**:
```java
// Flyweight interface
interface Flyweight {
    void operation(String extrinsicState);
}

// ConcreteFlyweight
class ConcreteFlyweight implements Flyweight {
    private String intrinsicState;

    public ConcreteFlyweight(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    public void operation(String extrinsicState) {
        System.out.println("IntrinsicState: " + intrinsicState + ", ExtrinsicState: " + extrinsicState);
    }
}

// FlyweightFactory
class FlyweightFactory {
    private Map<String, Flyweight> flyweights = new HashMap<>();

    public Flyweight getFlyweight(String intrinsicState) {
        Flyweight flyweight = flyweights.get(intrinsicState);
        if (flyweight == null) {
            flyweight = new ConcreteFlyweight(intrinsicState);
            flyweights.put(intrinsicState, flyweight);
        }
        return flyweight;
    }
}

// Client code
public class FlyweightExample {
    public static void main(String[] args) {
        FlyweightFactory factory = new FlyweightFactory();

        Flyweight flyweight1 = factory.getFlyweight("State1");
        flyweight1.operation("Context1");

        Flyweight flyweight2 = factory.getFlyweight("State1");
        flyweight2.operation("Context2");

        Flyweight flyweight3 = factory.getFlyweight("State2");
        flyweight3.operation("Context3");
        
        // Output:
        // IntrinsicState: State1, ExtrinsicState: Context1
        // IntrinsicState: State1, ExtrinsicState: Context2
        // IntrinsicState: State2, ExtrinsicState: Context3
    }
}
```

### 7. **Proxy**

#### **Use**:
- Provides a surrogate or placeholder for another object to control access to it, often for the purposes of lazy initialization, access control, or logging.
- Useful for controlling access to resources that are expensive to create or manage.

#### **Example**:
```java
// Subject interface
interface Subject {
    void request();
}

// RealSubject
class RealSubject implements Subject {
    public void request() {
        System.out.println("RealSubject request");
    }
}

// Proxy
class Proxy implements Subject {
    private RealSubject realSubject;

    public void request() {
        if (realSubject == null) {
            realSubject = new RealSubject();
        }
        realSubject.request();
    }
}

// Client code
public class ProxyExample {
    public static void main(String[] args) {
        Subject proxy = new Proxy();
        proxy.request();  // Output: RealSubject request
    }
}
```

## **Behavioral Design Patterns**

### 1. **Chain of Responsibility**

#### **Use**:
- Allows multiple objects to handle a request without the sender needing to know which object will handle it. Each handler can pass the request to the next handler in the chain.
- Useful for implementing a chain of processing steps or handling requests in a flexible manner.

#### **Example**:
```java
// Handler
abstract class Handler {
    private Handler nextHandler;

    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public void handleRequest(String request) {
        if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// ConcreteHandlerA
class ConcreteHandlerA extends Handler {
    public void handleRequest(String request) {
        if (request.equals("A")) {
            System.out.println("Handled by ConcreteHandlerA");
        } else {
            super.handleRequest(request);
        }
    }
}

// ConcreteHandlerB
class ConcreteHandlerB extends Handler {
    public void handleRequest(String request) {
        if (request.equals("B")) {
            System.out.println("Handled by ConcreteHandlerB");
        } else {
            super.handleRequest(request);
        }
    }
}

// Client code
public class ChainOfResponsibilityExample {
    public static void main(String[] args) {
        Handler handlerA = new ConcreteHandlerA();
        Handler handlerB = new ConcreteHandlerB();

        handlerA.setNextHandler(handlerB);

        handlerA.handleRequest("A");  // Output: Handled by ConcreteHandlerA
        handlerA.handleRequest("B");  // Output: Handled by ConcreteHandlerB
        handlerA.handleRequest("C");  // No output
    }
}
```

### 2. **Command**

#### **Use**:
- Encapsulates a request as an object, thereby allowing parameterization of clients with different requests, queuing of requests, and logging of the requests.
- Useful for implementing undo/redo functionality or queuing operations.

#### **Example**:
```java
// Command interface
interface Command {
    void execute();
}

// ConcreteCommand
class ConcreteCommand implements Command {
    private Receiver receiver;

    public ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    public void execute() {
        receiver.action();
    }
}

// Receiver
class Receiver {
    public void action() {
        System.out.println("Receiver action");
    }
}

// Invoker
class Invoker {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void invoke() {
        command.execute();
    }
}

// Client code
public class CommandExample {
    public static void main(String[] args) {
        Receiver receiver = new Receiver();
        Command command = new ConcreteCommand(receiver);
        Invoker invoker = new Invoker();
        invoker.setCommand(command);
        invoker.invoke();  // Output: Receiver action
    }
}
```

### 3. **Iterator**

#### **Use**:
- Provides a way to access elements of a collection sequentially without exposing its underlying representation.
- Useful for traversing collections or aggregate objects in a standard manner.

#### **Example**:
```java
// Iterator interface
interface Iterator {
    boolean hasNext();
    Object next();
}

// Iterable interface
interface IterableCollection {
    Iterator createIterator();
}

// ConcreteIterator
class ConcreteIterator implements Iterator {
    private String[] items;
    private int position;

    public ConcreteIterator(String[] items) {
        this.items = items;
        this.position = 0;
    }

    public boolean hasNext() {
        return position < items.length;
    }

    public Object next() {
        return items[position++];
    }
}

// ConcreteCollection
class ConcreteCollection implements IterableCollection {
    private String[] items;

    public ConcreteCollection(String[] items) {
        this.items = items;
    }

    public Iterator createIterator() {
        return new ConcreteIterator(items);
    }
}

// Client code
public class IteratorExample {
    public static void main(String[] args) {
        String[] items = {"Item1", "Item2", "Item3"};
        IterableCollection collection = new ConcreteCollection(items);
        Iterator iterator = collection.createIterator();

        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

### 4. **Mediator**

#### **Use**:
- Defines an object that encapsulates how a set of objects interact, promoting loose coupling by preventing objects from referring to each other explicitly.
- Useful for managing complex communications between multiple objects.

#### **Example**:
```java
// Mediator interface
interface Mediator {
    void send(String message, Colleague colleague);
}

// Colleague
abstract class Colleague {
    protected Mediator mediator;

    public Colleague(Mediator mediator) {
        this.mediator = mediator;
    }
}

// ConcreteColleague
class ConcreteColleague1 extends Colleague {
    public ConcreteColleague1(Mediator mediator) {
        super(mediator);
    }

    public void send(String message) {
        mediator.send(message, this);
    }

    public void receive(String message) {
        System.out.println("Colleague1 received: " + message);
    }
}

// ConcreteColleague
class ConcreteColleague2 extends Colleague {
    public ConcreteColleague2(Mediator mediator) {
        super(mediator);
    }

    public void receive(String message) {
        System.out.println("Colleague2 received: " + message);
    }
}

// ConcreteMediator
class ConcreteMediator implements Mediator {
    private ConcreteColleague1 colleague1;
    private ConcreteColleague2 colleague2;

    public void setColleague1(ConcreteColleague1 colleague1) {
        this.colleague1 = colleague1;
   

 }

    public void setColleague2(ConcreteColleague2 colleague2) {
        this.colleague2 = colleague2;
    }

    public void send(String message, Colleague sender) {
        if (sender == colleague1) {
            colleague2.receive(message);
        } else {
            colleague1.receive(message);
        }
    }
}

// Client code
public class MediatorExample {
    public static void main(String[] args) {
        ConcreteMediator mediator = new ConcreteMediator();
        ConcreteColleague1 colleague1 = new ConcreteColleague1(mediator);
        ConcreteColleague2 colleague2 = new ConcreteColleague2(mediator);

        mediator.setColleague1(colleague1);
        mediator.setColleague2(colleague2);

        colleague1.send("Hello from Colleague1");
        colleague2.send("Hello from Colleague2");
        // Output:
        // Colleague2 received: Hello from Colleague1
        // Colleague1 received: Hello from Colleague2
    }
}
```

### 5. **Memento**

#### **Use**:
- Captures and externalizes an object's internal state without violating encapsulation so that the object can be restored to that state later.
- Useful for implementing undo/redo functionality or preserving object states.

#### **Example**:
```java
// Memento
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

// Originator
class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public Memento createMemento() {
        return new Memento(state);
    }

    public void restoreMemento(Memento memento) {
        state = memento.getState();
    }

    public String getState() {
        return state;
    }
}

// Caretaker
class Caretaker {
    private Memento memento;

    public void saveMemento(Memento memento) {
        this.memento = memento;
    }

    public Memento getMemento() {
        return memento;
    }
}

// Client code
public class MementoExample {
    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        originator.setState("State1");
        caretaker.saveMemento(originator.createMemento());

        originator.setState("State2");
        System.out.println("Current State: " + originator.getState());  // Output: State2

        originator.restoreMemento(caretaker.getMemento());
        System.out.println("Restored State: " + originator.getState());  // Output: State1
    }
}
```

### 6. **Observer**

#### **Use**:
- Defines a dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
- Useful for implementing event handling systems or creating dynamic, decoupled applications.

#### **Example**:
```java
// Observer interface
interface Observer {
    void update(String message);
}

// Subject interface
interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// ConcreteSubject
class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String message;

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void setMessage(String message) {
        this.message = message;
        notifyObservers();
    }
}

// ConcreteObserver
class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " received message: " + message);
    }
}

// Client code
public class ObserverExample {
    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();
        Observer observer1 = new ConcreteObserver("Observer1");
        Observer observer2 = new ConcreteObserver("Observer2");

        subject.addObserver(observer1);
        subject.addObserver(observer2);

        subject.setMessage("Hello Observers");
        // Output:
        // Observer1 received message: Hello Observers
        // Observer2 received message: Hello Observers
    }
}
```

### 7. **State**

#### **Use**:
- Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
- Useful for implementing state machines or objects that need to transition between different states.

#### **Example**:
```java
// State interface
interface State {
    void handleRequest();
}

// ConcreteStateA
class ConcreteStateA implements State {
    public void handleRequest() {
        System.out.println("Handling request in State A");
    }
}

// ConcreteStateB
class ConcreteStateB implements State {
    public void handleRequest() {
        System.out.println("Handling request in State B");
    }
}

// Context
class Context {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public void request() {
        state.handleRequest();
    }
}

// Client code
public class StateExample {
    public static void main(String[] args) {
        Context context = new Context();

        context.setState(new ConcreteStateA());
        context.request();  // Output: Handling request in State A

        context.setState(new ConcreteStateB());
        context.request();  // Output: Handling request in State B
    }
}
```

### 8. **Strategy**

#### **Use**:
- Defines a family of algorithms, encapsulates each algorithm, and makes them interchangeable. The strategy pattern lets the algorithm vary independently from clients that use it.
- Useful for implementing different algorithms or strategies in a flexible and reusable manner.

#### **Example**:
```java
// Strategy interface
interface Strategy {
    int execute(int a, int b);
}

// ConcreteStrategyA
class ConcreteStrategyA implements Strategy {
    public int execute(int a, int b) {
        return a + b;
    }
}

// ConcreteStrategyB
class ConcreteStrategyB implements Strategy {
    public int execute(int a, int b) {
        return a * b;
    }
}

// Context
class Context {
    private Strategy strategy;

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public int executeStrategy(int a, int b) {
        return strategy.execute(a, b);
    }
}

// Client code
public class StrategyExample {
    public static void main(String[] args) {
        Context context = new Context();

        context.setStrategy(new ConcreteStrategyA());
        System.out.println("Result: " + context.executeStrategy(5, 3));  // Output: Result: 8

        context.setStrategy(new ConcreteStrategyB());
        System.out.println("Result: " + context.executeStrategy(5, 3));  // Output: Result: 15
    }
}
```

### 9. **Template Method**

#### **Use**:
- Defines the skeleton of an algorithm in a base class but lets subclasses override specific steps of the algorithm without changing its structure.
- Useful for defining a template for operations where some steps can be customized.

#### **Example**:
```java
// AbstractClass
abstract class AbstractClass {
    // Template method
    public final void templateMethod() {
        step1();
        step2();
        step3();
    }

    // Concrete methods
    private void step1() {
        System.out.println("Step 1");
    }

    // Abstract methods
    protected abstract void step2();
    protected abstract void step3();
}

// ConcreteClassA
class ConcreteClassA extends AbstractClass {
    protected void step2() {
        System.out.println("ConcreteClassA Step 2");
    }

    protected void step3() {
        System.out.println("ConcreteClassA Step 3");
    }
}

// ConcreteClassB
class ConcreteClassB extends AbstractClass {
    protected void step2() {
        System.out.println("ConcreteClassB Step 2");
    }

    protected void step3() {
        System.out.println("ConcreteClassB Step 3");
    }
}

// Client code
public class TemplateMethodExample {
    public static void main(String[] args) {
        AbstractClass classA = new ConcreteClassA();
        classA.templateMethod();
        // Output:
        // Step 1
        // ConcreteClassA Step 2
        // ConcreteClassA Step 3

        AbstractClass classB = new ConcreteClassB();
        classB.templateMethod();
        // Output:
        // Step 1
        // ConcreteClassB Step 2
        // ConcreteClassB Step 3
    }
}
```

### 10. **Visitor**

#### **Use**:
- Defines a new operation to a set of objects without changing the classes of the objects.
- Useful for operations that need to be performed on elements of an object structure but don’t want to alter the elements themselves.

#### **Example**:
```java
// Visitor interface
interface Visitor {
    void visit(Element element);
}

// ConcreteVisitor
class ConcreteVisitor implements Visitor {
    public void visit(Element element) {
        System.out.println("Visiting " + element);
    }
}

// Element interface
interface Element {
    void accept(Visitor visitor);
}

// ConcreteElement
class ConcreteElement implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }

    @Override
    public String toString() {
        return "ConcreteElement";
    }
}

// Client code
public class VisitorExample {
    public static void main(String[] args)

 {
        Element element = new ConcreteElement();
        Visitor visitor = new ConcreteVisitor();

        element.accept(visitor);  // Output: Visiting ConcreteElement
    }
}
```

I hope this helps you with understanding and implementing these design patterns in your Java projects! If you have any more questions or need further clarification, feel free to ask.
