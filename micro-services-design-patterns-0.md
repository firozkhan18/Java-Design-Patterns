Microservices design patterns are strategies and best practices used to effectively develop, deploy, and manage microservices architectures. Microservices architecture is an approach where a large application is built as a suite of small, independently deployable services, each responsible for a specific business function. Here are some common design patterns used in microservices architectures, along with their uses and examples:

### 1. **API Gateway**

**Use:** Acts as a single entry point for all client requests, routing them to the appropriate microservice. It also handles cross-cutting concerns like authentication, logging, and rate limiting.

**Example:** In an e-commerce application, instead of having clients interact directly with the product, user, and order services, they would communicate with an API Gateway. The gateway would route requests to the appropriate service and aggregate responses if needed.

### 2. **Service Discovery**

**Use:** Helps services find and communicate with each other dynamically. When a service needs to communicate with another, it queries the service registry to find the address of the target service.

**Example:** In a travel booking application, if the booking service needs to contact the payment service, it would query a service registry to obtain the current address of the payment service, which might change over time.

### 3. **Circuit Breaker**

**Use:** Protects services from cascading failures by detecting failures and preventing the system from making repeated requests to a failing service. It allows the system to recover gracefully.

**Example:** If the inventory service in a retail application is down, the circuit breaker pattern prevents other services from repeatedly attempting to access the inventory service and potentially worsening the situation.

### 4. **Bulkhead**

**Use:** Isolates failures to prevent them from affecting the entire system. It divides the system into isolated pools to limit the impact of failures.

**Example:** In a video streaming application, different services like user management, video playback, and recommendations are isolated. If the recommendation service fails, it won’t affect the video playback service or user management.

### 5. **Saga**

**Use:** Manages distributed transactions across multiple microservices by breaking them into a series of smaller, isolated transactions that are coordinated through compensation actions if necessary.

**Example:** In an online order system, a saga might involve a sequence of steps like payment processing, inventory update, and shipment. If any step fails, compensation actions are triggered to rollback changes made in previous steps.

### 6. **Event Sourcing**

**Use:** Stores the state of a system as a sequence of events rather than a current snapshot. Each change to the system is captured as an event, and the state is rebuilt by replaying these events.

**Example:** In a financial application, instead of storing the current balance of an account, you store all transactions (deposits and withdrawals). The current balance can be computed by replaying these transaction events.

### 7. **CQRS (Command Query Responsibility Segregation)**

**Use:** Separates the read and write operations into different models. This helps optimize performance, scalability, and security for each type of operation.

**Example:** In an inventory management system, the command side handles updates to stock levels, while the query side handles reading stock levels for display purposes. This separation allows each side to be optimized independently.

### 8. **Strangler Fig**

**Use:** Facilitates incremental migration from a monolithic system to a microservices architecture by gradually replacing parts of the monolith with new microservices.

**Example:** For a content management system, you might start by creating a new microservice for user authentication while keeping the rest of the system as is. Over time, more features are migrated to microservices until the entire system is modernized.

### 9. **Database per Service**

**Use:** Each microservice has its own database, ensuring that services are loosely coupled and can be scaled independently.

**Example:** In a social media application, the user service might have its own database for user profiles, while the messaging service has a separate database for storing messages. This isolation allows each service to be optimized and scaled according to its own needs.

### 10. **Shared Database**

**Use:** Allows multiple microservices to access the same database schema, which can be simpler but tightly couples services to the database structure.

**Example:** In a small-scale application where services are tightly coupled, multiple services like user management and orders might share the same database to simplify data consistency. However, this approach can lead to scalability and maintainability challenges as the system grows.

### 11. **Decomposition by Business Capability**

**Use:** Decomposes an application into microservices based on business capabilities rather than technical layers.

**Example:** In an online marketplace, you might decompose the application into services like product catalog, shopping cart, user management, and order processing, each representing a distinct business capability.

By leveraging these design patterns, organizations can build robust, scalable, and maintainable microservices architectures that meet their specific needs and challenges.

Certainly! Here’s an example code snippet for each of the mentioned microservices design patterns using Java. These examples provide a basic understanding of how each pattern can be implemented.

### 1. **API Gateway**

**Use:** Acts as a single entry point for all client requests, routing them to the appropriate microservice.

**Example Code:**

Using Spring Boot to implement an API Gateway:

```java
// ApiGatewayApplication.java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ApiGatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}

// GatewayController.java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
@RequestMapping("/api")
public class GatewayController {

    private final RestTemplate restTemplate = new RestTemplate();

    @Value("${user.service.url}")
    private String userServiceUrl;

    @GetMapping("/users/{id}")
    public String getUser(@PathVariable String id) {
        return restTemplate.getForObject(userServiceUrl + "/users/" + id, String.class);
    }
}
```

**Note:** You would need to configure `user.service.url` in `application.properties` and add dependencies for Spring Cloud Gateway for a fully-featured API Gateway.

### 2. **Service Discovery**

**Use:** Allows services to find each other dynamically.

**Example Code:**

Using Spring Cloud Netflix Eureka for service discovery:

**Eureka Server:**

```java
// EurekaServerApplication.java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

**Service Registration:**

```java
// UserServiceApplication.java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```

### 3. **Circuit Breaker**

**Use:** Protects a service from cascading failures.

**Example Code:**

Using Resilience4j for circuit breaker:

```java
// UserService.java
import io.github.resilience4j.circuitbreaker.CircuitBreaker;
import io.github.resilience4j.circuitbreaker.CircuitBreakerRegistry;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class UserService {

    private final RestTemplate restTemplate = new RestTemplate();
    private final CircuitBreaker circuitBreaker;

    public UserService(CircuitBreakerRegistry registry) {
        this.circuitBreaker = registry.circuitBreaker("userServiceCircuitBreaker");
    }

    public String getUser(String id) {
        return CircuitBreaker.decorateSupplier(circuitBreaker, () -> restTemplate.getForObject("http://user-service/users/" + id, String.class)).get();
    }
}
```

### 4. **Bulkhead**

**Use:** Isolates failures to prevent them from affecting the entire system.

**Example Code:**

Using Resilience4j for bulkhead:

```java
// UserService.java
import io.github.resilience4j.bulkhead.Bulkhead;
import io.github.resilience4j.bulkhead.BulkheadRegistry;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class UserService {

    private final RestTemplate restTemplate = new RestTemplate();
    private final Bulkhead bulkhead;

    public UserService(BulkheadRegistry registry) {
        this.bulkhead = registry.bulkhead("userServiceBulkhead");
    }

    public String getUser(String id) {
        return Bulkhead.decorateSupplier(bulkhead, () -> restTemplate.getForObject("http://user-service/users/" + id, String.class)).get();
    }
}
```

### 5. **Saga**

**Use:** Manages distributed transactions across multiple microservices.

**Example Code:**

Simple Saga orchestration using Spring Boot and RabbitMQ for communication:

**Saga Orchestrator:**

```java
// OrderService.java
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.stereotype.Service;

@Service
public class OrderService {

    private final RabbitTemplate rabbitTemplate;

    public OrderService(RabbitTemplate rabbitTemplate) {
        this.rabbitTemplate = rabbitTemplate;
    }

    public void createOrder(String orderId) {
        try {
            // Send message to charge service
            rabbitTemplate.convertAndSend("orderExchange", "charge", orderId);
            // Send message to inventory service
            rabbitTemplate.convertAndSend("orderExchange", "inventory", orderId);
        } catch (Exception e) {
            // Send compensation messages if something fails
            rabbitTemplate.convertAndSend("orderExchange", "compensate", orderId);
        }
    }
}
```

### 6. **Event Sourcing**

**Use:** Stores the state of a system as a sequence of events.

**Example Code:**

**Event Store:**

```java
// Event.java
public class Event {
    private String type;
    private String data;

    // Getters and setters
}

// EventService.java
import org.springframework.stereotype.Service;

@Service
public class EventService {

    private final List<Event> eventStore = new ArrayList<>();

    public void storeEvent(Event event) {
        eventStore.add(event);
        System.out.println("Event stored: " + event);
    }

    public List<Event> getAllEvents() {
        return eventStore;
    }
}
```

### 7. **CQRS (Command Query Responsibility Segregation)**

**Use:** Separates read and write operations into different models.

**Example Code:**

**Command Service:**

```java
// CommandService.java
import org.springframework.stereotype.Service;

@Service
public class CommandService {

    public void updateStock(String productId, int quantity) {
        // Logic to update stock
        System.out.println("Stock updated for productId: " + productId);
    }
}
```

**Query Service:**

```java
// QueryService.java
import org.springframework.stereotype.Service;

@Service
public class QueryService {

    public String getStock(String productId) {
        // Logic to get stock
        return "Stock for productId: " + productId;
    }
}
```

### 8. **Strangler Fig**

**Use:** Gradually replaces parts of a monolithic system with microservices.

**Example Code:**

**Old Monolithic Endpoint:**

```java
// OldMonolithController.java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class OldMonolithController {

    @GetMapping("/api/users/{id}")
    public String getUser(@PathVariable String id) {
        // Old logic
        return "User from monolith: " + id;
    }
}
```

**New Microservice Endpoint:**

```java
// UserMicroserviceController.java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserMicroserviceController {

    @GetMapping("/users/{id}")
    public String getUser(@PathVariable String id) {
        // New microservice logic
        return "User from microservice: " + id;
    }
}
```

### 9. **Database per Service**

**Use:** Each microservice has its own database.

**Example Code:**

**User Service:**

```java
// UserService.java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUser(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
    }
}

// UserRepository.java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

**Order Service:**

```java
// OrderService.java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Service;

@Service
public class OrderService {

    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public Order getOrder(Long id) {
        return orderRepository.findById(id).orElseThrow(() -> new RuntimeException("Order not found"));
    }
}

// OrderRepository.java
import org.springframework.data.jpa.repository.JpaRepository;

public interface OrderRepository extends JpaRepository<Order, Long> {
}
```

### 10. **Shared Database**

**Use:** Allows multiple microservices to access the same database schema.

**Example Code:**

**Shared Database:**

**User and Order Services using the same database:**

```java
// UserService.java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUser(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
    }
}

// OrderService.java
import org.springframework.data.jpa.repository

.JpaRepository;
import org.springframework.stereotype.Service;

@Service
public class OrderService {

    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public Order getOrder(Long id) {
        return orderRepository.findById(id).orElseThrow(() -> new RuntimeException("Order not found"));
    }
}
```

### 11. **Decomposition by Business Capability**

**Use:** Decomposes an application into microservices based on business capabilities.

**Example Code:**

**Product Catalog Service:**

```java
// ProductService.java
import org.springframework.stereotype.Service;

@Service
public class ProductService {

    public String getProduct(String id) {
        // Logic to get product details
        return "Product details for: " + id;
    }
}
```

**Order Processing Service:**

```java
// OrderProcessingService.java
import org.springframework.stereotype.Service;

@Service
public class OrderProcessingService {

    public String processOrder(String orderId) {
        // Logic to process order
        return "Order processed: " + orderId;
    }
}
```

Each microservice focuses on a specific business capability, such as managing products or processing orders.

These examples demonstrate basic implementations of each design pattern using Java. For production systems, additional considerations such as security, configuration management, and comprehensive error handling would be necessary.
