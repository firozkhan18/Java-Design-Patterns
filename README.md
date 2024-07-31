Certainly! Let's delve into the concepts with code examples to illustrate the topics related to Java and Spring Boot:

### 1. Why is `1 == 1` True but Values Greater than 127 are False When Dealing with Wrapper Classes in Java?

In Java, `Integer` objects between `-128` and `127` are cached to optimize memory. Therefore, `Integer` objects within this range are shared, and comparing them with `==` will return `true` if they are the same value.

```java
public class Main {
    public static void main(String[] args) {
        Integer a = 100;
        Integer b = 100;
        System.out.println(a == b); // true, cached value

        Integer c = 200;
        Integer d = 200;
        System.out.println(c == d); // false, new instances
    }
}
```

### 2. Implement Your Own Rule-Engine in Spring Boot

A simple rule engine can be implemented using the Strategy pattern. Here’s a basic example:

```java
public interface Rule {
    boolean apply(int value);
}

public class EvenRule implements Rule {
    @Override
    public boolean apply(int value) {
        return value % 2 == 0;
    }
}

public class RuleEngine {
    private List<Rule> rules = new ArrayList<>();

    public void addRule(Rule rule) {
        rules.add(rule);
    }

    public boolean evaluate(int value) {
        for (Rule rule : rules) {
            if (!rule.apply(value)) {
                return false;
            }
        }
        return true;
    }
}
```

### 3. Which of the Following is Not a Primitive Data Type in Java?

In Java, primitive types are `int`, `char`, `double`, `float`, `boolean`, `byte`, `short`, and `long`. `Integer` is not a primitive type; it’s a wrapper class.

### 4. When to Use a Parallel Stream in Java?

Parallel streams are useful for CPU-intensive tasks with large datasets. Here’s an example:

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // Sequential stream
        long start = System.currentTimeMillis();
        numbers.stream().map(n -> n * n).forEach(System.out::println);
        System.out.println("Sequential Time: " + (System.currentTimeMillis() - start));

        // Parallel stream
        start = System.currentTimeMillis();
        numbers.parallelStream().map(n -> n * n).forEach(System.out::println);
        System.out.println("Parallel Time: " + (System.currentTimeMillis() - start));
    }
}
```

### 5. How is `takeWhile` and `dropWhile` Different from `filter`?

`takeWhile` and `dropWhile` are short-circuiting operations introduced in Java 9. 

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

        // takeWhile: takes elements while the condition is true
        System.out.println(numbers.stream().takeWhile(n -> n < 4).toList()); // [1, 2, 3]

        // dropWhile: skips elements while the condition is true
        System.out.println(numbers.stream().dropWhile(n -> n < 4).toList()); // [4, 5, 6]

        // filter: filters elements based on the condition
        System.out.println(numbers.stream().filter(n -> n < 4).toList()); // [1, 2, 3]
    }
}
```

### 6. Java Code Common Problems

Common problems include `NullPointerException` and `ArrayIndexOutOfBoundsException`. Here’s how to handle a `NullPointerException`:

```java
public class Main {
    public static void main(String[] args) {
        String str = null;

        try {
            System.out.println(str.length()); // Throws NullPointerException
        } catch (NullPointerException e) {
            System.out.println("Caught NullPointerException");
        }
    }
}
```

### 7. Java 8 Coding Interview Questions

**Lambda Expression Example:**

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

        names.forEach(name -> System.out.println(name));
    }
}
```

**Optional Example:**

```java
import java.util.Optional;

public class Main {
    public static void main(String[] args) {
        Optional<String> optionalName = Optional.of("Alice");

        optionalName.ifPresent(name -> System.out.println("Name: " + name));
    }
}
```

### 8. Improve Your Code Readability In Project Using Spring Boot and Java

Use meaningful names, follow coding standards, and use comments:

```java
public class UserService {
    
    // Retrieves a user by their ID
    public User getUserById(Long userId) {
        // Implementation
    }
}
```

### 9. Pessimistic Lock in Spring Boot

Using `@Transactional` with `PESSIMISTIC_WRITE` lock:

```java
import org.springframework.transaction.annotation.Transactional;

@Transactional
public User getUserWithLock(Long userId) {
    return userRepository.findById(userId, LockModeType.PESSIMISTIC_WRITE).orElseThrow();
}
```

### 10. Is `Optional` Class Methods Compulsory in Java?

No, using `Optional` is not compulsory, but it helps to avoid `NullPointerException`:

```java
public class Main {
    public static void main(String[] args) {
        Optional<String> optionalValue = Optional.ofNullable(getValue());

        // Using Optional to avoid null checks
        optionalValue.ifPresent(value -> System.out.println("Value: " + value));
    }

    private static String getValue() {
        return null; // Example
    }
}
```

### 11. Stop Using `@Value` Annotation in Spring Boot

Replace `@Value` with `@ConfigurationProperties`:

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    // getters and setters
}
```

### 12. CommandLineRunner Interface in Spring Boot

Run code after the Spring Boot application starts:

```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MainApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        System.out.println("Application started with command-line arguments: " + Arrays.toString(args));
    }
}
```

### 13. How to Fix “Filename Too Long Error” During Git Pull

Adjust Git configuration:

```bash
git config --system core.longpaths true
```

### 14. How Do You Handle Checked Exceptions in Stream Pipeline?

Wrap checked exceptions:

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        List<String> fileNames = Arrays.asList("file1.txt", "file2.txt");

        fileNames.stream()
            .map(fileName -> {
                try {
                    return readFile(fileName);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            })
            .forEach(System.out::println);
    }

    private static String readFile(String fileName) throws IOException {
        // Implementation
        return "";
    }
}
```

### 15. Best Practice of Java Stream in Your Project

Avoid side effects, use parallel streams for large datasets, and prefer method chaining:

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.stream()
    .filter(name -> name.startsWith("A"))
    .map(String::toUpperCase)
    .forEach(System.out::println);
```

### 16. How to Implement Custom Collector Using Java Stream?

Implement a custom collector:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collector;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

        List<String> collectedNames = names.stream()
            .collect(Collectors.collectingAndThen(
                Collectors.toList(),
                list -> {
                    list.add("Additional Element");
                    return list;
                }
            ));

        System.out.println(collectedNames);
    }
}
```

### 17. Log Time Taken By REST API in Spring Boot Project to Verify the Application Performance

Using an interceptor to log request time:

```java
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.time.Instant;

@Component
public class PerformanceInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        request.setAttribute("startTime", Instant.now());
        return true;
    }

    @Override
   

 public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        Instant startTime = (Instant) request.getAttribute("startTime");
        Instant endTime = Instant.now();
        long duration = endTime.toEpochMilli() - startTime.toEpochMilli();
        System.out.println("Request took " + duration + "ms");
    }
}
```

### 18. How to Handle Application Deployment Configuration?

Use Spring Boot profiles:

```properties
# application-dev.properties
server.port=8081

# application-prod.properties
server.port=80
```

Specify the profile when starting the application:

```bash
java -jar myapp.jar --spring.profiles.active=prod
```

### 19. How to Generate API Documentation Using Swagger?

Add Swagger dependencies and configure:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
    
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
            .select()
            .apis(RequestHandlerSelectors.any())
            .paths(PathSelectors.any())
            .build();
    }
}
```

### 20. How to Handle Multithreading Issues in Java?

Use `synchronized` and concurrent utilities:

```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

### 21. Best Practice of Exception Handling in Your Spring Boot Project

Use a global exception handler:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

### 22. How Do You Handle and Fix Vulnerabilities in Your Spring Boot Project?

Use tools like OWASP Dependency-Check, regularly update dependencies, and follow security best practices:

- Regularly update dependencies.
- Use tools for security analysis.
- Implement proper authentication and authorization.

### 23. How to Improve the Security of Your Spring Boot Application?

- Use HTTPS.
- Apply security headers.
- Validate and sanitize input.
- Implement authentication and authorization.

### 24. How to Implement Caching in Spring Boot?

Use `@Cacheable` annotation:

```java
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Cacheable("users")
    public User getUserById(Long userId) {
        // Fetch user from database
    }
}
```

Ensure you have the cache configuration in place:

```java
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableCaching
public class CacheConfig {
}
```

### 25. How to Write Unit Tests in Spring Boot?

Use `@SpringBootTest` for integration tests and Mockito for unit tests:

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest
public class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testGetUser() throws Exception {
        mockMvc.perform(get("/users/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("Alice"));
    }
}
```

### Kafka Consumer Example Using Java

To consume messages from a Kafka topic using Java, you need to set up a Kafka consumer with the required configurations. Here's a basic example:

1. **Add Dependencies**

   First, add Kafka dependencies to your `pom.xml` (if you're using Maven):

   ```xml
   <dependency>
       <groupId>org.apache.kafka</groupId>
       <artifactId>kafka-clients</artifactId>
       <version>3.1.0</version> <!-- Use the latest stable version -->
   </dependency>
   ```

2. **Kafka Consumer Code**

   Here’s a simple example of a Kafka consumer:

   ```java
   import org.apache.kafka.clients.consumer.ConsumerConfig;
   import org.apache.kafka.clients.consumer.KafkaConsumer;
   import org.apache.kafka.clients.consumer.ConsumerRecord;
   import org.apache.kafka.common.serialization.StringDeserializer;

   import java.util.Collections;
   import java.util.Properties;

   public class KafkaConsumerExample {
       public static void main(String[] args) {
           // Kafka Consumer configuration
           Properties props = new Properties();
           props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
           props.put(ConsumerConfig.GROUP_ID_CONFIG, "my-consumer-group");
           props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
           props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
           props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

           // Create Kafka Consumer
           KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);

           // Subscribe to a topic
           consumer.subscribe(Collections.singletonList("my-topic"));

           // Poll for new records
           while (true) {
               consumer.poll(100).forEach(record -> {
                   System.out.printf("Received message: (key: %s, value: %s, partition: %d, offset: %d)%n",
                           record.key(), record.value(), record.partition(), record.offset());
               });
           }
       }
   }
   ```

### Kafka Producer Example Using Java

To produce messages to a Kafka topic using Java, follow these steps:

1. **Add Dependencies**

   Ensure you have Kafka producer dependencies in your `pom.xml`:

   ```xml
   <dependency>
       <groupId>org.apache.kafka</groupId>
       <artifactId>kafka-clients</artifactId>
       <version>3.1.0</version> <!-- Use the latest stable version -->
   </dependency>
   ```

2. **Kafka Producer Code**

   Here’s a basic example of a Kafka producer:

   ```java
   import org.apache.kafka.clients.producer.KafkaProducer;
   import org.apache.kafka.clients.producer.ProducerConfig;
   import org.apache.kafka.clients.producer.ProducerRecord;
   import org.apache.kafka.common.serialization.StringSerializer;

   import java.util.Properties;

   public class KafkaProducerExample {
       public static void main(String[] args) {
           // Kafka Producer configuration
           Properties props = new Properties();
           props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
           props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
           props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

           // Create Kafka Producer
           KafkaProducer<String, String> producer = new KafkaProducer<>(props);

           // Produce messages
           for (int i = 0; i < 10; i++) {
               ProducerRecord<String, String> record = new ProducerRecord<>("my-topic", "key-" + i, "value-" + i);
               producer.send(record, (metadata, exception) -> {
                   if (exception != null) {
                       exception.printStackTrace();
                   } else {
                       System.out.printf("Produced record to topic %s partition %d with offset %d%n",
                               metadata.topic(), metadata.partition(), metadata.offset());
                   }
               });
           }

           // Close the producer
           producer.close();
       }
   }
   ```

### How To Install Apache Kafka On Windows?

Here’s a step-by-step guide to install and run Apache Kafka on Windows:

1. **Download and Install Kafka**

   - Download Kafka from [Apache Kafka Downloads](https://kafka.apache.org/downloads). Choose the latest binary release.
   - Extract the downloaded zip file to a directory of your choice.

2. **Install and Configure Zookeeper**

   Kafka requires Zookeeper to run. Kafka comes with a built-in Zookeeper. To start Zookeeper:

   - Open a command prompt and navigate to the Kafka directory.
   - Run the Zookeeper server:

     ```bash
     .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
     ```

3. **Install and Configure Kafka**

   - Open a new command prompt and navigate to the Kafka directory.
   - Start the Kafka server:

     ```bash
     .\bin\windows\kafka-server-start.bat .\config\server.properties
     ```

4. **Create a Kafka Topic**

   - Open another command prompt and navigate to the Kafka directory.
   - Use the Kafka topics command to create a topic:

     ```bash
     .\bin\windows\kafka-topics.bat --create --topic my-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
     ```

5. **Verify the Kafka Setup**

   - Produce a message to the topic:

     ```bash
     .\bin\windows\kafka-console-producer.bat --topic my-topic --bootstrap-server localhost:9092
     ```

     Type a few messages and press Enter.

   - Consume messages from the topic:

     ```bash
     .\bin\windows\kafka-console-consumer.bat --topic my-topic --from-beginning --bootstrap-server localhost:9092
     ```

You should see the messages you produced in the consumer console.

With these instructions, you should be able to set up and use Kafka on Windows, and write both producer and consumer applications in Java. 

Understanding how a Spring Boot application works internally involves exploring several key components and processes. Here’s a detailed breakdown of the internal workings of a Spring Boot application:

### 1. **Application Entry Point**

In a Spring Boot application, the entry point is typically the `main` method in a class annotated with `@SpringBootApplication`. This class is responsible for launching the application.

**Example:**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class, args);
    }
}
```

- `@SpringBootApplication` is a convenience annotation that combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`.

### 2. **SpringApplication Initialization**

When you call `SpringApplication.run()`, several key processes occur:

- **Bootstrap Phase:** Initializes the application context and loads the initial configurations.
- **Context Initialization:** Sets up the Spring ApplicationContext, which is the central interface to the Spring IoC (Inversion of Control) container.
- **Bean Factory Initialization:** Creates and configures beans defined in the application context.

### 3. **Auto-Configuration**

Spring Boot’s auto-configuration feature tries to automatically configure your Spring application based on the dependencies you have added. It is powered by `spring.factories` files located in JARs of Spring Boot Starter dependencies.

**Key Points:**

- **Conditional Configuration:** Uses `@Conditional` annotations to apply configurations based on certain conditions (e.g., classpath presence, environment properties).
- **Spring Boot Starter Dependencies:** Include predefined configurations and libraries for various use cases (e.g., web development, data access).

### 4. **Component Scanning**

Spring Boot performs component scanning to detect and register beans in the application context. This is done through the `@ComponentScan` annotation, which is included in `@SpringBootApplication`.

**Component Scanning:** 

- Scans for classes annotated with `@Component`, `@Service`, `@Repository`, `@Controller`, etc.
- Automatically detects and registers these components as beans in the Spring context.

### 5. **Configuration and Properties**

Spring Boot applications can be configured using properties files (`application.properties` or `application.yml`) or via code.

- **External Configuration:** Properties files can be used to set configurations outside the application code.
- **Profile-Specific Configuration:** Spring Boot allows different configurations for different environments using profiles (e.g., `application-dev.properties`, `application-prod.properties`).

### 6. **Dependency Injection**

Spring’s core feature is dependency injection (DI). It allows for the creation and management of bean instances and their dependencies:

- **Constructor Injection:** Preferred method where dependencies are injected via the class constructor.
- **Setter Injection:** Dependencies are injected via setter methods.
- **Field Injection:** Dependencies are injected directly into fields (less preferred due to lack of immutability).

### 7. **Application Context**

The ApplicationContext is the core container of the Spring framework. It manages the lifecycle of beans and provides various services, including:

- **Bean Lifecycle Management:** Creation, initialization, and destruction of beans.
- **Event Publishing:** Mechanism to publish and listen to application events.
- **Resource Loading:** Access to resources like properties files and classpath resources.

### 8. **Embedded Server**

Spring Boot applications can include an embedded web server (e.g., Tomcat, Jetty) to run the application as a standalone service:

- **Embedded Tomcat/Jetty:** When you use Spring Boot starters for web applications, an embedded server is included and configured automatically.
- **Configuration:** The server can be configured via properties or programmatically.

### 9. **Endpoints and Controllers**

Spring Boot provides an easy way to define RESTful web services and MVC controllers:

- **REST Controllers:** Defined using `@RestController` and handle HTTP requests.
- **Request Mappings:** Use `@RequestMapping` or shortcut annotations (`@GetMapping`, `@PostMapping`) to map HTTP requests to methods.

**Example:**

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

### 10. **Actuators**

Spring Boot includes Actuator modules that provide production-ready features like monitoring and management:

- **Endpoints:** Expose information about application health, metrics, environment properties, etc.
- **Custom Endpoints:** You can create custom actuator endpoints for your specific needs.

### 11. **Security**

Spring Boot integrates with Spring Security to provide authentication and authorization features:

- **Configuration:** Security can be configured using `SecurityConfig` class or through properties.
- **Default Security:** Spring Boot includes default security configurations that can be customized.

### 12. **Application Shutdown**

When a Spring Boot application shuts down:

- **Context Closing:** The ApplicationContext is closed, triggering the destruction of beans.
- **Graceful Shutdown:** Managed by Spring Boot to ensure resources are released and cleanup operations are performed.

### Summary

Here’s a concise summary of how a Spring Boot application works internally:

1. **Application Startup:** Initiated by `SpringApplication.run()`.
2. **Auto-Configuration:** Automatically configures components based on dependencies.
3. **Component Scanning:** Detects and registers beans.
4. **Dependency Injection:** Manages bean dependencies.
5. **Application Context:** Central container for beans and services.
6. **Embedded Server:** Provides web server functionality.
7. **Endpoints and Controllers:** Handles HTTP requests.
8. **Actuators:** Exposes management and monitoring endpoints.
9. **Security:** Provides authentication and authorization.
10. **Shutdown:** Closes the context and performs cleanup.

Understanding these components helps in designing, developing, and troubleshooting Spring Boot applications effectively.

In Java, the `HashSet` class is a commonly used implementation of the `Set` interface. It provides a collection that does not allow duplicate elements. The internal mechanism that ensures this uniqueness involves the use of a `HashMap` for storage and leveraging hashing to manage element uniqueness. Here’s a detailed explanation of how `HashSet` works internally:

### 1. **Internal Data Structure**

`HashSet` internally uses a `HashMap` to store its elements. The `HashMap` is a key-value map where the keys are the elements of the `HashSet`, and the values are dummy objects (typically a constant `Object` or `Boolean.TRUE`). This usage is efficient for managing the uniqueness of elements and provides constant time complexity (`O(1)`) for basic operations like add, remove, and contains.

**Key Points:**
- **HashMap Usage:** Elements of the `HashSet` are stored as keys in a `HashMap`, and the associated values are irrelevant. The internal `HashMap` handles the underlying storage and hashing mechanism.
- **Dummy Values:** Since `HashSet` is only concerned with uniqueness and not with value storage, dummy values are used in the `HashMap`.

### 2. **Hashing and Bucket Storage**

- **Hash Function:** When an element is added to the `HashSet`, the `HashMap` computes the hash code of the element using the `hashCode()` method. This hash code is then used to determine the bucket in which the element will be stored.
- **Buckets:** Internally, a `HashMap` maintains an array of buckets (or lists). Each bucket corresponds to a hash code value, and elements with the same hash code are stored in the same bucket.

**Hashing Process:**
1. **Compute Hash Code:** Use the `hashCode()` method of the element to compute its hash code.
2. **Bucket Selection:** The hash code is used to select a bucket (i.e., index) in the underlying array.
3. **Bucket Handling:** If there are multiple elements in the same bucket (i.e., hash collisions), the bucket is typically implemented as a linked list or a balanced tree (in Java 8 and later) to handle these collisions.

### 3. **Maintaining Uniqueness**

- **Equality Check:** When adding an element, the `HashSet` uses the `equals()` method to ensure that the element is not already present in the set. This is done within the bucket where the element would be stored.
- **Handling Duplicates:** If an element with the same hash code and `equals()` method returns `true` (indicating that the element is already present in the bucket), the `HashSet` does not add the new element and maintains the uniqueness.

**Example:**

```java
import java.util.HashSet;

public class HashSetExample {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();

        set.add("Apple");
        set.add("Banana");
        set.add("Apple");  // Duplicate element

        System.out.println(set);  // Output: [Apple, Banana]
    }
}
```

In the above example, the `HashSet` will not add the second "Apple" because it already exists in the set.

### 4. **Performance Considerations**

- **Time Complexity:** Operations like add, remove, and contains typically have a constant time complexity (`O(1)`) due to the hash-based mechanism.
- **Space Complexity:** Space complexity is dependent on the number of elements and the capacity of the underlying `HashMap`. The load factor (default is 0.75) determines when the `HashMap` will resize its internal array to maintain performance.

### 5. **Hashing Collisions**

- **Handling Collisions:** Hash collisions occur when two different elements have the same hash code. In such cases, elements are stored in the same bucket and are managed using a linked list or tree structure within that bucket.

### 6. **Resizing**

- **Dynamic Resizing:** The capacity of the `HashMap` (and hence `HashSet`) is dynamically adjusted. When the number of elements exceeds a certain threshold (determined by the load factor), the `HashMap` resizes its internal array to maintain efficiency. This resizing involves rehashing the existing elements into a new array with more buckets.

**Example of Resizing:**

- When the number of elements exceeds 75% of the current bucket array size, the `HashMap` creates a new larger array, recalculates hash codes, and redistributes elements into the new buckets.

### Summary

**How `HashSet` Ensures Uniqueness:**

1. **HashMap-Based Storage:** Uses a `HashMap` where elements are stored as keys.
2. **Hashing and Buckets:** Elements are distributed into buckets based on their hash codes.
3. **Uniqueness by Equality Check:** Uses `equals()` method to check for duplicates within a bucket.
4. **Handling Collisions:** Manages hash collisions using lists or trees within buckets.
5. **Resizing:** Dynamically resizes and rehashes to maintain performance.

Understanding these internal mechanisms helps in effectively using `HashSet` and diagnosing performance issues related to hash-based collections.

Sure! Here’s a breakdown of each of these tasks with Java 8 code examples:

### 1. **Find the First Non-Repeated Character in a String**

To find the first non-repeated character in a string using Java 8 Stream API, you can follow these steps:

**Steps:**
1. Convert the string to a stream of characters.
2. Use a `LinkedHashMap` to maintain the insertion order while counting occurrences.
3. Filter and find the first character that appears exactly once.

**Code Example:**

```java
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.Optional;

public class FirstNonRepeatedCharacter {
    public static void main(String[] args) {
        String input = "swiss";

        Optional<Character> firstNonRepeated = input.chars()
                .mapToObj(c -> (char) c)
                .collect(LinkedHashMap::new,
                        (map, c) -> map.put(c, map.getOrDefault(c, 0) + 1),
                        LinkedHashMap::putAll)
                .entrySet()
                .stream()
                .filter(entry -> entry.getValue() == 1)
                .map(Map.Entry::getKey)
                .findFirst();

        firstNonRepeated.ifPresent(System.out::println); // Output: w
    }
}
```

**Explanation:**
- Convert the string to an `IntStream` of character codes, then map it to `Character`.
- Collect characters into a `LinkedHashMap` to maintain the insertion order and count occurrences.
- Filter out the characters that occur only once and find the first such character.

### 2. **Filter Out Duplicate Characters in a String Using Java 8**

To filter out duplicate characters from a string, use the `distinct()` method of streams:

**Code Example:**

```java
import java.util.Arrays;
import java.util.stream.Collectors;

public class RemoveDuplicateCharacters {
    public static void main(String[] args) {
        String input = "aabbccddeeff";

        String result = input.chars()
                .mapToObj(c -> (char) c)
                .distinct()
                .map(String::valueOf)
                .collect(Collectors.joining());

        System.out.println(result); // Output: abcdef
    }
}
```

**Explanation:**
- Convert the string to an `IntStream` of character codes and map it to `Character`.
- Use `distinct()` to filter out duplicate characters.
- Collect the unique characters back to a string.

### 3. **Count Occurrence of Each Character in a String Using Java 8**

To count occurrences of each character in a string, you can use the `Collectors.groupingBy()` collector:

**Code Example:**

```java
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class CountCharacterOccurrences {
    public static void main(String[] args) {
        String input = "hello";

        Map<Character, Long> characterCount = input.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        characterCount.forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```

**Explanation:**
- Convert the string to a stream of characters.
- Use `Collectors.groupingBy()` to group characters and count their occurrences using `Collectors.counting()`.
- Print the results.

### 4. **Sort a List in Java Using Java 8**

Java 8 provides several ways to sort lists:

**1. Using `List.sort()` with a Comparator:**

**Code Example:**

```java
import java.util.Arrays;
import java.util.List;

public class SortList {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry", "date");

        list.sort(String::compareTo);

        list.forEach(System.out::println); // Output: apple, banana, cherry, date
    }
}
```

**2. Using Streams with `sorted()`:**

**Code Example:**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class SortListWithStreams {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry", "date");

        List<String> sortedList = list.stream()
                .sorted()
                .collect(Collectors.toList());

        sortedList.forEach(System.out::println); // Output: apple, banana, cherry, date
    }
}
```

**3. Sorting with Custom Comparator:**

**Code Example:**

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class SortListWithComparator {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry", "date");

        list.sort(Comparator.reverseOrder());

        list.forEach(System.out::println); // Output: date, cherry, banana, apple
    }
}
```

**Explanation:**
- `List.sort()` directly sorts the list using a comparator or natural order.
- Streams provide a functional approach to sorting and can be used to create new sorted collections.
- Custom comparators allow for more complex sorting logic, such as reverse order or other criteria.

Here's a detailed guide to tackle each of the topics you've listed:

### 1. **Sort a Map Using Java 8 Stream API**

To sort a `Map` by its values or keys, you can use the `Stream` API. Here's how you can do both:

**Sort by Key:**

```java
import java.util.*;
import java.util.stream.Collectors;

public class SortMapByKey {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 5);
        map.put("banana", 2);
        map.put("cherry", 7);

        Map<String, Integer> sortedMap = map.entrySet()
                .stream()
                .sorted(Map.Entry.comparingByKey())
                .collect(Collectors.toMap(
                        Map.Entry::getKey,
                        Map.Entry::getValue,
                        (e1, e2) -> e1,
                        LinkedHashMap::new));

        sortedMap.forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```

**Sort by Value:**

```java
import java.util.*;
import java.util.stream.Collectors;

public class SortMapByValue {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 5);
        map.put("banana", 2);
        map.put("cherry", 7);

        Map<String, Integer> sortedMap = map.entrySet()
                .stream()
                .sorted(Map.Entry.comparingByValue())
                .collect(Collectors.toMap(
                        Map.Entry::getKey,
                        Map.Entry::getValue,
                        (e1, e2) -> e1,
                        LinkedHashMap::new));

        sortedMap.forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```

**Explanation:**
- `Map.entrySet().stream()` creates a stream of map entries.
- `sorted()` sorts the entries by key or value.
- `collect(Collectors.toMap())` collects the sorted entries into a `LinkedHashMap` to maintain the order.

### 2. **Sort a List Using Java 8 Stream API**

Sorting a list can be done using `Stream.sorted()`:

**Code Example:**

```java
import java.util.*;
import java.util.stream.Collectors;

public class SortList {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("banana", "apple", "cherry", "date");

        List<String> sortedList = list.stream()
                .sorted()
                .collect(Collectors.toList());

        sortedList.forEach(System.out::println); // Output: apple, banana, cherry, date
    }
}
```

**Explanation:**
- `list.stream()` creates a stream of the list.
- `sorted()` sorts the elements.
- `collect(Collectors.toList())` collects the sorted elements into a new list.

### 3. **Java 8 Map, Filter, and Reduce Methods**

**Map:** Transforms each element using a function.

**Filter:** Filters elements based on a predicate.

**Reduce:** Reduces elements to a single value using a binary operator.

**Code Example:**

```java
import java.util.Arrays;
import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

public class MapFilterReduce {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // Map: square each number
        List<Integer> squared = numbers.stream()
                .map(n -> n * n)
                .collect(Collectors.toList());

        System.out.println("Squared: " + squared);

        // Filter: even numbers only
        List<Integer> evens = numbers.stream()
                .filter(n -> n % 2 == 0)
                .collect(Collectors.toList());

        System.out.println("Even numbers: " + evens);

        // Reduce: sum of all numbers
        Optional<Integer> sum = numbers.stream()
                .reduce(Integer::sum);

        System.out.println("Sum: " + sum.orElse(0));
    }
}
```

**Explanation:**
- `map()` applies a function to each element.
- `filter()` excludes elements that do not match the predicate.
- `reduce()` combines elements using a binary operator.

### 4. **Sequential Stream vs Parallel Stream in Java**

**Sequential Stream:** Processes elements one at a time in a single thread.

**Parallel Stream:** Processes elements concurrently using multiple threads.

**Code Example:**

```java
import java.util.Arrays;
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // Sequential stream
        long startTime = System.nanoTime();
        numbers.stream().forEach(n -> System.out.println(n + " processed by " + Thread.currentThread().getName()));
        long endTime = System.nanoTime();
        System.out.println("Sequential time: " + (endTime - startTime) + " ns");

        // Parallel stream
        startTime = System.nanoTime();
        numbers.parallelStream().forEach(n -> System.out.println(n + " processed by " + Thread.currentThread().getName()));
        endTime = System.nanoTime();
        System.out.println("Parallel time: " + (endTime - startTime) + " ns");
    }
}
```

**Explanation:**
- `stream()` processes elements sequentially.
- `parallelStream()` processes elements in parallel.

### 5. **Java 8 Optional Best Practices**

**Optional** is used to represent optional values that may or may not be present.

**Best Practices:**
- Use `Optional.of()` or `Optional.ofNullable()` to create instances.
- Avoid using `Optional.get()` directly; use `ifPresent()` or `orElse()` instead.
- Use `map()` and `flatMap()` to apply transformations.

**Code Example:**

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> optional = Optional.ofNullable("Hello");

        // Using ifPresent
        optional.ifPresent(System.out::println);

        // Using orElse
        String result = optional.orElse("Default Value");
        System.out.println(result);

        // Using map
        Optional<String> upper = optional.map(String::toUpperCase);
        upper.ifPresent(System.out::println);
    }
}
```

### 6. **Split a String Only on the First Occurrence of Delimiter**

**Code Example:**

```java
public class SplitString {
    public static void main(String[] args) {
        String input = "one,two,three,four";
        String[] parts = input.split(",", 2);

        for (String part : parts) {
            System.out.println(part);
        }
    }
}
```

**Explanation:**
- The second parameter in `split()` specifies the limit, i.e., split into a maximum of `n` parts.

### 7. **Which of the Following is Not a Primitive Data Type in Java?**

**Answer:** Non-primitive data types include classes, interfaces, and arrays. Primitive types include `int`, `float`, `double`, `char`, `boolean`, `byte`, `short`, and `long`.

### 8. **When to Use a Parallel Stream in Java?**

Use parallel streams when:
- You have large collections and complex operations that can benefit from parallel processing.
- The tasks are CPU-bound and the operations are independent of each other.

**Code Example:**

```java
import java.util.Arrays;
import java.util.List;

public class ParallelStreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        numbers.parallelStream()
                .map(n -> n * n)
                .forEach(System.out::println);
    }
}
```

### 9. **How `takeWhile` and `dropWhile` Differ from `filter`**

**takeWhile**: Takes elements while the predicate is true, stops as soon as the predicate is false.

**dropWhile**: Drops elements while the predicate is true, starts returning elements when the predicate becomes false.

**filter**: Applies the predicate to all elements and returns the elements that match.

**Code Example:**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class TakeDropFilter {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);

        List<Integer> takeWhile = numbers.stream()
                .takeWhile(n -> n < 5)
                .collect(Collectors.toList());
        System.out.println("takeWhile: " + takeWhile); // [1, 2, 3, 4]

        List<Integer> dropWhile = numbers.stream()
                .dropWhile(n -> n < 5)
                .collect(Collectors.toList());
        System.out.println("dropWhile: " + dropWhile); // [5, 6, 7, 8, 9]

        List<Integer> filter = numbers.stream()
                .filter(n -> n < 5)
                .collect(Collectors.toList());
        System.out.println("filter: " + filter); // [1, 2, 3, 4]
    }
}
```

### 10. **Java Code Common Problems**

**NullPointerException (NPE):**
- Occurs when you access or modify an object reference that is

 `null`.

**Solution:** Use Optional, proper null checks, or initialize objects before use.

### 11. **Is Optional Class Methods Compulsory in Java?**

**Answer:** No, using `Optional` methods is not compulsory but highly recommended to avoid `NullPointerException` and write more readable and maintainable code.

### 12. **Handle Checked Exceptions in Stream Pipeline**

Checked exceptions can be handled in streams by using custom wrappers or utility methods.

**Code Example:**

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class CheckedExceptionHandling {
    public static void main(String[] args) {
        List<String> lines = Arrays.asList("1", "2", "a");

        lines.stream()
                .map(CheckedExceptionHandling::parseInt)
                .forEach(System.out::println);
    }

    private static int parseInt(String s) {
        try {
            return Integer.parseInt(s);
        } catch (NumberFormatException e) {
            return 0; // Handle exception, e.g., default value or log
        }
    }
}
```

### 13. **Best Practice of Java Stream in Your Project**

- Use streams for data manipulation and transformation.
- Avoid using streams for small data sets where traditional loops are more efficient.
- Make use of lazy evaluation to improve performance.

### 14. **How to Implement Custom Collector Using Java Stream**

**Code Example:**

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collector;
import java.util.stream.Collectors;

public class CustomCollectorExample {
    public static void main(String[] args) {
        List<String> strings = List.of("a", "b", "c", "d");

        List<String> collected = strings.stream()
                .collect(Collectors.toCollection(ArrayList::new));

        System.out.println(collected);
    }
}
```

**Explanation:**
- Implementing custom collectors involves extending `Collector` interface and defining methods for accumulation, combination, and finishing.

### 15. **Remove If-Else from Your Code**

Refactor if-else statements using polymorphism or strategy patterns for cleaner code.

**Code Example:**

```java
public interface Strategy {
    void execute();
}

public class ConcreteStrategyA implements Strategy {
    public void execute() {
        System.out.println("Strategy A");
    }
}

public class ConcreteStrategyB implements Strategy {
    public void execute() {
        System.out.println("Strategy B");
    }
}

public class Context {
    private Strategy strategy;

    public Context(Strategy strategy) {
        this.strategy = strategy;
    }

    public void executeStrategy() {
        strategy.execute();
    }
}

public class Main {
    public static void main(String[] args) {
        Strategy strategy = new ConcreteStrategyA();
        Context context = new Context(strategy);
        context.executeStrategy();

        strategy = new ConcreteStrategyB();
        context = new Context(strategy);
        context.executeStrategy();
    }
}
```

### 16. **Happy Numbers in Java**

A happy number is a number where repeatedly summing the squares of its digits eventually leads to 1.

**Code Example:**

```java
public class HappyNumber {
    public static void main(String[] args) {
        System.out.println(isHappy(19)); // true
    }

    private static boolean isHappy(int n) {
        Set<Integer> seen = new HashSet<>();
        while (n != 1 && !seen.contains(n)) {
            seen.add(n);
            n = sumOfSquares(n);
        }
        return n == 1;
    }

    private static int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
}
```

### 17. **Get Current Date and Time Using Java 8 Date and Time API**

**Code Example:**

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class DateTimeExample {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();
        System.out.println("Current Date and Time: " + now);

        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String formattedDate = now.format(formatter);
        System.out.println("Formatted Date and Time: " + formattedDate);
    }
}
```

### 18. **Find the Longest String in an Array of Strings**

**Code Example:**

```java
import java.util.Arrays;

public class LongestString {
    public static void main(String[] args) {
        String[] array = {"short", "medium", "a much longer string"};

        String longest = Arrays.stream(array)
                .max((s1, s2) -> Integer.compare(s1.length(), s2.length()))
                .orElse("No strings available");

        System.out.println("Longest string: " + longest);
    }
}
```

### 19. **Find the Second Highest Element in an Array Using Java 8**

**Code Example:**

```java
import java.util.Arrays;

public class SecondHighestElement {
    public static void main(String[] args) {
        int[] array = {1, 3, 4, 2, 5};

        int secondHighest = Arrays.stream(array)
                .distinct()
                .sorted()
                .skip(array.length - 2)
                .findFirst()
                .orElseThrow(() -> new RuntimeException("Not enough elements"));

        System.out.println("Second highest element: " + secondHighest);
    }
}
```

### 20. **Count Frequency of a String in Java Using Stream**

**Code Example:**

```java
import java.util.Arrays;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class CountFrequency {
    public static void main(String[] args) {
        String input = "hello world";

        Map<Character, Long> frequency = input.chars()
                .mapToObj(c -> (char) c)
                .filter(c -> c != ' ')
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        frequency.forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```

### 21. **Best Way to Reverse a String in Java**

**Code Example:**

```java
public class ReverseString {
    public static void main(String[] args) {
        String original = "hello";

        // Using StringBuilder
        String reversed = new StringBuilder(original).reverse().toString();
        System.out.println("Reversed: " + reversed);

        // Using Stream
        String reversedStream = new String(original.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.toList()).toString());
        System.out.println("Reversed using stream: " + reversedStream);
    }
}
```
**Implementing a Rule Engine in Spring Boot**

Creating a custom rule engine involves defining a set of rules and the logic to execute them. Here’s a step-by-step guide to implementing a basic rule engine in a Spring Boot application.

### **1. Project Setup**

**Step 1: Initialize a Spring Boot Project**
- Use [Spring Initializr](https://start.spring.io/) to create a new Spring Boot project with dependencies like Spring Web and Spring Boot DevTools.

**Step 2: Download and Unzip**
- Generate the project, download the zip file, and extract it. Open the project in your favorite IDE.

### **2. Define Rule Engine Components**

**Step 1: Create a Rule Interface**

Define a `Rule` interface that all rules will implement.

```java
package com.example.ruleengine.rules;

public interface Rule {
    boolean evaluate(Object context);
}
```

**Step 2: Implement Specific Rules**

Create concrete implementations of the `Rule` interface. For example, a rule that checks if a user’s age is above 18.

```java
package com.example.ruleengine.rules;

public class AgeRule implements Rule {
    private int minAge;

    public AgeRule(int minAge) {
        this.minAge = minAge;
    }

    @Override
    public boolean evaluate(Object context) {
        if (context instanceof Integer) {
            return (Integer) context >= minAge;
        }
        return false;
    }
}
```

**Step 3: Create a Rule Engine**

Implement the rule engine that will execute a list of rules.

```java
package com.example.ruleengine.engine;

import com.example.ruleengine.rules.Rule;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
public class RuleEngine {
    public boolean evaluateRules(List<Rule> rules, Object context) {
        for (Rule rule : rules) {
            if (!rule.evaluate(context)) {
                return false;
            }
        }
        return true;
    }
}
```

**Step 4: Define a Controller**

Create a REST controller to test the rule engine.

```java
package com.example.ruleengine.controller;

import com.example.ruleengine.engine.RuleEngine;
import com.example.ruleengine.rules.AgeRule;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.Arrays;
import java.util.List;

@RestController
public class RuleController {
    @Autowired
    private RuleEngine ruleEngine;

    @GetMapping("/validate")
    public String validateAge(@RequestParam int age) {
        List<AgeRule> rules = Arrays.asList(new AgeRule(18));
        boolean isValid = ruleEngine.evaluateRules((List) (List<? extends Rule>) rules, age);
        return isValid ? "Age is valid" : "Age is not valid";
    }
}
```

### **3. Running the Application**

**Step 1: Run Spring Boot Application**

Run the application using your IDE or command line.

**Step 2: Test the Rule Engine**

Open your browser or use a tool like `curl` or Postman to test the endpoint:

```
http://localhost:8080/validate?age=20
```

You should get a response indicating whether the age is valid according to the rule.

---

**Java 8 Coding Interview Questions**

Here are some common Java 8 coding interview questions and answers:

**Question 1: How to find the first non-repeated character in a string?**

**Solution:**

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class FirstNonRepeatedCharacter {
    public static char findFirstNonRepeated(String str) {
        Map<Character, Integer> charCountMap = new LinkedHashMap<>();
        
        for (char c : str.toCharArray()) {
            charCountMap.put(c, charCountMap.getOrDefault(c, 0) + 1);
        }
        
        for (Map.Entry<Character, Integer> entry : charCountMap.entrySet()) {
            if (entry.getValue() == 1) {
                return entry.getKey();
            }
        }
        
        throw new RuntimeException("No non-repeated character found");
    }

    public static void main(String[] args) {
        String str = "swiss";
        System.out.println("First non-repeated character: " + findFirstNonRepeated(str));
    }
}
```

**Question 2: How to filter out duplicate characters from a string?**

**Solution:**

```java
import java.util.stream.Collectors;

public class RemoveDuplicates {
    public static String removeDuplicates(String str) {
        return str.chars()
                  .distinct()
                  .mapToObj(c -> String.valueOf((char) c))
                  .collect(Collectors.joining());
    }

    public static void main(String[] args) {
        String str = "programming";
        System.out.println("String without duplicates: " + removeDuplicates(str));
    }
}
```

**Question 3: Count occurrences of each character in a string using Java 8 Streams.**

**Solution:**

```java
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class CharacterFrequency {
    public static Map<Character, Long> countCharacterFrequency(String str) {
        return str.chars()
                  .mapToObj(c -> (char) c)
                  .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }

    public static void main(String[] args) {
        String str = "character";
        System.out.println("Character frequency: " + countCharacterFrequency(str));
    }
}
```

**Question 4: Find the longest string in an array of strings.**

**Solution:**

```java
import java.util.Arrays;

public class LongestString {
    public static String findLongestString(String[] strings) {
        return Arrays.stream(strings)
                     .max((s1, s2) -> Integer.compare(s1.length(), s2.length()))
                     .orElseThrow(() -> new RuntimeException("Array is empty"));
    }

    public static void main(String[] args) {
        String[] strings = {"short", "longer", "longest"};
        System.out.println("Longest string: " + findLongestString(strings));
    }
}
```

**Question 5: Find the second highest element in an array.**

**Solution:**

```java
import java.util.Arrays;
import java.util.stream.IntStream;

public class SecondHighest {
    public static int findSecondHighest(int[] numbers) {
        return IntStream.of(numbers)
                        .distinct()
                        .boxed()
                        .sorted((a, b) -> b - a)
                        .skip(1)
                        .findFirst()
                        .orElseThrow(() -> new RuntimeException("Not enough distinct elements"));
    }

    public static void main(String[] args) {
        int[] numbers = {3, 5, 7, 2, 5, 9, 7};
        System.out.println("Second highest number: " + findSecondHighest(numbers));
    }
}
```

### **Improving Code Readability in Spring Boot**

Improving code readability in Spring Boot involves:
1. **Consistent Naming Conventions**: Use meaningful names for classes, methods, and variables.
2. **Modularization**: Break down large classes into smaller, focused components.
3. **Clear Documentation**: Use comments and Javadoc to explain the purpose of classes and methods.
4. **Error Handling**: Implement global exception handling to avoid cluttering business logic with error handling code.
5. **Refactoring**: Regularly refactor code to improve structure and readability.
6. **Unit Tests**: Write unit tests to ensure that code changes do not introduce bugs and to clarify expected behavior.

### **Pessimistic Locking in Spring Boot**

**What is Pessimistic Locking?**

Pessimistic locking is a strategy where a record is locked for update when it is read. This prevents other transactions from modifying or accessing the record until the lock is released.

**How to Apply Pessimistic Locking in Spring Boot:**

1. **Entity Definition:**

   ```java
   @Entity
   public class Account {
       @Id
       private Long id;

       @Version
       private Long version;

       private BigDecimal balance;

       // Getters and Setters
   }
   ```

2. **Repository Interface:**

   ```java
   import org.springframework.data.jpa.repository.Lock;
   import org.springframework.data.jpa.repository.JpaRepository;
   import org.springframework.data.jpa.repository.Query;
   import javax.persistence.LockModeType;

   public interface AccountRepository extends JpaRepository<Account, Long> {
       @Lock(LockModeType.PESSIMISTIC_WRITE)
       @Query("select a from Account a where a.id = :id")
       Account findByIdForUpdate(Long id);
   }
   ```

3. **Service Layer:**

   ```java
   @Service
   public class AccountService {
       @Autowired
       private AccountRepository accountRepository;

       @Transactional
       public void updateAccountBalance(Long accountId, BigDecimal amount) {
           Account account = accountRepository.findByIdForUpdate(accountId);
           account.setBalance(account.getBalance().add(amount));
           accountRepository.save(account);
       }
   }
   ```

### **Additional Topics**

**1. CommandLineRunner Interface in Spring Boot**

The `CommandLineRunner` interface allows you to run specific code after the application context is initialized.

**Example:**

```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class MyCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("Application started with command-line arguments: " + Arrays.toString(args));
   

 }
}
```

**2. Fixing “Filename too long error” During Git Pull**

To fix this error on Windows, run:

```sh
git config --system core.longpaths true
```

**3. Logging Time Taken by REST API**

Use Spring Boot’s `@ControllerAdvice` and `StopWatch` to log API execution time:

```java
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.concurrent.TimeUnit;

@Component
public class PerformanceInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        request.setAttribute("startTime", System.currentTimeMillis());
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        long startTime = (Long) request.getAttribute("startTime");
        long endTime = System.currentTimeMillis();
        long executeTime = endTime - startTime;
        System.out.println("Request URL: " + request.getRequestURL().toString() + " | Time Taken: " + executeTime + "ms");
    }
}
```

**4. Logging Correlation ID**

Implement a `Filter` to add a correlation ID to each log entry:

```java
import org.slf4j.MDC;
import org.springframework.stereotype.Component;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;
import java.util.UUID;

@Component
public class CorrelationIdFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        String correlationId = UUID.randomUUID().toString();
        MDC.put("correlationId", correlationId);
        try {
            chain.doFilter(request, response);
        } finally {
            MDC.remove("correlationId");
        }
    }

    @Override
    public void init(FilterConfig filterConfig) {}

    @Override
    public void destroy() {}
}
```

**5. Spring Boot + OpenAI ChatGPT Integration**

To integrate with ChatGPT, you can use OpenAI’s API:

1. **Add Dependency**: Include an HTTP client library in your `pom.xml`.

2. **Service Implementation**:

   ```java
   @Service
   public class ChatGPTService {
       private final String apiKey = "your-api-key";

       public String chat(String prompt) {
           // Code to call OpenAI's API using HttpClient
       }
   }
   ```

3. **Controller**:

   ```java
   @RestController
   public class ChatGPTController {
       @Autowired
       private ChatGPTService chatGPTService;

       @GetMapping("/chat")
       public ResponseEntity<String> chat(@RequestParam String prompt) {
           return ResponseEntity.ok(chatGPTService.chat(prompt));
       }
   }
   ```

**6. Spring Boot Logging Best Practices**

- Use appropriate log levels (`DEBUG`, `INFO`, `WARN`, `ERROR`).
- Centralize logging configuration using `logback-spring.xml`.
- Avoid logging sensitive information.

**7. Using StringUtils Methods**

Apache Commons Lang `StringUtils` provides utility methods for string manipulation:

```java
import org.apache.commons.lang3.StringUtils;

public class StringUtilsExample {
    public static void main(String[] args) {
        String str = "   Hello World!   ";
        System.out.println(StringUtils.trim(str));
    }
}
```

**8. Generic API Response Model**

Define a generic response structure:

```java
public class ApiResponse<T> {
    private T data;
    private String message;
    private boolean success;

    // Constructor, getters, and setters
}
```

Use in controllers:

```java
@GetMapping("/items")
public ApiResponse<List<Item>> getItems() {
    List<Item> items = itemService.getAllItems();
    return new ApiResponse<>(items, "Items retrieved successfully", true);
}
```

**9. Spring Data REST Tutorial**

Spring Data REST automatically exposes repository methods as RESTful endpoints. Use `@RepositoryRestResource` to customize:

```java
@RepositoryRestResource
public interface ItemRepository extends JpaRepository<Item, Long> {
}
```

**10. Spring Boot MapStruct**

MapStruct simplifies object mapping between DTOs and entities:

1. **Add Dependency**:

   ```xml
   <dependency>
       <groupId>org.mapstruct</groupId>
       <artifactId>mapstruct</artifactId>
       <version>latest-version</version>
   </dependency>
   ```

2. **Define Mapper Interface**:

   ```java
   @Mapper
   public interface ItemMapper {
       ItemDTO toDTO(Item item);
       Item toEntity(ItemDTO itemDTO);
   }
   ```

**11. Handling OutOfMemoryError**

**Solution**:
- Increase heap size using JVM options (`-Xmx`).
- Optimize memory usage.

**12. Fixing “Java class file has wrong version”**

Ensure compatibility between Java versions. Update libraries to match your Java version.

---
### How To Resolve CORS Issues in a Spring Boot Application

CORS (Cross-Origin Resource Sharing) issues often arise when a web application makes requests to a server hosted on a different domain, protocol, or port. In a Spring Boot application, these issues can be resolved by configuring CORS settings to allow requests from specific origins.

Here’s a step-by-step guide to resolve CORS issues in a Spring Boot application:

---

#### **1. Understanding CORS**

When your Angular UI makes a request to your Spring Boot backend API, it might encounter CORS issues if the backend is not configured to allow requests from the origin of the Angular application. This issue occurs because browsers enforce the Same-Origin Policy, which restricts web pages from making requests to a different domain.

---

#### **2. Adding CORS Configuration in Spring Boot**

You can resolve CORS issues in Spring Boot by configuring global CORS settings using a `@Configuration` class. Follow these steps:

**Step 1: Create a Configuration Class**

Create a new configuration class and annotate it with `@Configuration`. This class will define CORS mappings globally.

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**") // Allow all paths
                .allowedOrigins("http://localhost:4200") // Your Angular UI URL
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

**Step 2: Explanation of Configuration**

- `addMapping("/**")`: Applies the CORS configuration to all endpoints.
- `allowedOrigins("http://localhost:4200")`: Specifies which origin is allowed. Change this URL to match the URL of your Angular application. You can also specify multiple origins by providing an array of URLs.
- `allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")`: Lists the HTTP methods allowed for CORS requests.
- `allowedHeaders("*")`: Allows all headers in CORS requests.
- `allowCredentials(true)`: Allows credentials (like cookies or authorization headers) to be included in the requests.

---

#### **3. Using `@CrossOrigin` Annotation**

Alternatively, you can configure CORS on a per-controller or per-method basis using the `@CrossOrigin` annotation.

**Example of applying `@CrossOrigin` at the controller level:**

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
@CrossOrigin(origins = "http://localhost:4200")
public class MessageController {

    @GetMapping("/message")
    public String getMessage() {
        return "Hello, World!";
    }
}
```

**Example of applying `@CrossOrigin` at the method level:**

```java
@GetMapping("/message")
@CrossOrigin(origins = "http://localhost:4200")
public String getMessage() {
    return "Hello, World!";
}
```

This approach is useful if you only need to enable CORS for specific endpoints rather than globally.

---

#### **4. Testing Your Configuration**

After adding CORS configurations, restart your Spring Boot application and test the API calls from your Angular application. If configured correctly, the CORS issues should be resolved, and your Angular application should be able to communicate with the Spring Boot backend without encountering CORS errors.

---

### Summary

In summary, resolving CORS issues in a Spring Boot application involves configuring CORS settings either globally using a `@Configuration` class or locally using the `@CrossOrigin` annotation. This ensures that requests from your Angular UI to your Spring Boot backend are permitted and handled correctly.


It looks like you’re working on a Spring Boot configuration class that uses properties to set CORS (Cross-Origin Resource Sharing) settings dynamically. This approach can be useful for externalizing CORS settings into configuration files, making it easier to modify them without changing the code.

Here’s a complete example of how you can use `@Value` annotations to configure CORS settings dynamically based on properties defined in your application configuration files:

### Full Example of CORS Configuration Using Properties

**1. Define Properties in `application.properties` or `application.yml`:**

First, you need to define your CORS settings in the `application.properties` or `application.yml` file. For example, in `application.properties`:

```properties
cors.allowedOrigins=http://localhost:4200
cors.allowedMethods=GET,POST,PUT,DELETE,OPTIONS
cors.allowedHeaders=*
cors.allowCredentials=true
```

Or, in `application.yml`:

```yaml
cors:
  allowedOrigins: http://localhost:4200
  allowedMethods: GET,POST,PUT,DELETE,OPTIONS
  allowedHeaders: "*"
  allowCredentials: true
```

**2. Create the `SecurityConfiguration` Class:**

Next, you’ll use these properties in your Spring Boot configuration class. Here’s how you can do it:

```java
package com.example.demo.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class SecurityConfiguration implements WebMvcConfigurer {

    @Value("${cors.allowedOrigins}")
    private String[] allowedOrigins;

    @Value("${cors.allowedMethods}")
    private String allowedMethods;

    @Value("${cors.allowedHeaders}")
    private String allowedHeaders;

    @Value("${cors.allowCredentials}")
    private boolean allowCredentials;

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins(allowedOrigins)
                .allowedMethods(allowedMethods.split(","))
                .allowedHeaders(allowedHeaders.split(","))
                .allowCredentials(allowCredentials);
    }
}
```

**3. Explanation:**

- **`@Value` Annotations:**
  - `@Value("${cors.allowedOrigins}")` injects the value of the `cors.allowedOrigins` property. It is assumed that this value is a comma-separated list, so it’s split into an array.
  - `@Value("${cors.allowedMethods}")` injects the allowed HTTP methods. This value is also comma-separated.
  - `@Value("${cors.allowedHeaders}")` injects the allowed headers. This value can include multiple headers separated by commas.
  - `@Value("${cors.allowCredentials}")` injects a boolean value indicating whether credentials are allowed in CORS requests.

- **`addCorsMappings(CorsRegistry registry)`:** Configures global CORS settings. It maps the CORS configuration to all paths (`/**`) and uses the values injected from properties to set allowed origins, methods, headers, and credentials.

---

### Additional Considerations

- **Error Handling:** Ensure that the property values are correctly formatted in your `application.properties` or `application.yml` files to avoid runtime errors.
- **Environment Specific Configurations:** If you need different CORS configurations for different environments (development, production), you can use Spring Profiles to manage environment-specific configurations.

By following these steps, you can dynamically configure CORS settings in your Spring Boot application based on properties defined in your configuration files. This approach makes it easier to manage and adjust CORS settings without modifying the codebase directly.


Sort List in Java using JAVA 8 different ways.
Gain Java Knowledge
Gain Java Knowledge

·
Follow

2 min read
·
Jul 16, 2023
3




During a project development , every developer comes across a situation where implementation of sorting becomes mandatory. In this article we will mainly focus on sorting using java 8.

Example 1 : Sort a list of String alphabetically.

List<String> names = Arrays.asList("Vivek", "nasim", "Sumit", "gaurav", "Mohit");
Print names in sorting order , now here we will use Comparator to sort a list.

names.sort(Comparator.naturalOrder());
OUTPUT : sorting with natural order : [Mohit, Sumit, Vivek, gaurav, nasim]

naturalOrder() method first place all capital letter names after that small case letter names. So, we can use CASE_INSENSITIVE_ORDER, which return a case insensitive order.

names.sort(String.CASE_INSENSITIVE_ORDER);
OUTPUT : sorting with case Insenesitive : [gaurav, Mohit, nasim, Sumit, Vivek]

Sort A List of Integer :

List<Integer> numbers = Arrays.asList(10, 8, 5, 2, 13, 2);
numbers.sort(Comparator.naturalOrder());
Sorting With Lambda Expression :

List<Employee> employees = Arrays.asList(
        new Employee("Sumit", 18, 2000),
        new Employee("Karan", 23, 6000),
        new Employee("Arjun", 25, 4000)
);

employees.sort((Employee e1, Employee e2) ->
        e1.getName().compareTo(e2.getName()));
OUTPUT …


Sorting lists in Java 8 can be accomplished using several methods. Here’s an overview of different ways to sort lists in Java 8:

### Example 1: Sorting a List of Strings Alphabetically

#### Using `Comparator.naturalOrder()`
```java
import java.util.*;

public class SortExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Vivek", "nasim", "Sumit", "gaurav", "Mohit");
        names.sort(Comparator.naturalOrder());
        System.out.println("Sorting with natural order: " + names);
    }
}
```
**Output:**
```
Sorting with natural order: [Mohit, Sumit, Vivek, gaurav, nasim]
```

#### Using `String.CASE_INSENSITIVE_ORDER`
```java
import java.util.*;

public class SortExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Vivek", "nasim", "Sumit", "gaurav", "Mohit");
        names.sort(String.CASE_INSENSITIVE_ORDER);
        System.out.println("Sorting with case insensitive order: " + names);
    }
}
```
**Output:**
```
Sorting with case insensitive order: [gaurav, Mohit, nasim, Sumit, Vivek]
```

### Example 2: Sorting a List of Integers

#### Using `Comparator.naturalOrder()`
```java
import java.util.*;

public class SortExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(10, 8, 5, 2, 13, 2);
        numbers.sort(Comparator.naturalOrder());
        System.out.println("Sorting integers: " + numbers);
    }
}
```
**Output:**
```
Sorting integers: [2, 2, 5, 8, 10, 13]
```

### Example 3: Sorting a List of Custom Objects Using Lambda Expressions

#### Sorting by Name
```java
import java.util.*;

class Employee {
    private String name;
    private int age;
    private double salary;

    public Employee(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public double getSalary() {
        return salary;
    }

    @Override
    public String toString() {
        return name + " (" + age + ", " + salary + ")";
    }
}

public class SortExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Sumit", 18, 2000),
                new Employee("Karan", 23, 6000),
                new Employee("Arjun", 25, 4000)
        );

        employees.sort((e1, e2) -> e1.getName().compareTo(e2.getName()));
        System.out.println("Sorting by name: " + employees);
    }
}
```
**Output:**
```
Sorting by name: [Arjun (25, 4000.0), Karan (23, 6000.0), Sumit (18, 2000.0)]
```

#### Sorting by Age
```java
import java.util.*;

public class SortExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Sumit", 18, 2000),
                new Employee("Karan", 23, 6000),
                new Employee("Arjun", 25, 4000)
        );

        employees.sort(Comparator.comparingInt(Employee::getAge));
        System.out.println("Sorting by age: " + employees);
    }
}
```
**Output:**
```
Sorting by age: [Sumit (18, 2000.0), Karan (23, 6000.0), Arjun (25, 4000.0)]
```

#### Sorting by Salary in Descending Order
```java
import java.util.*;

public class SortExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Sumit", 18, 2000),
                new Employee("Karan", 23, 6000),
                new Employee("Arjun", 25, 4000)
        );

        employees.sort((e1, e2) -> Double.compare(e2.getSalary(), e1.getSalary()));
        System.out.println("Sorting by salary (descending): " + employees);
    }
}
```
**Output:**
```
Sorting by salary (descending): [Karan (23, 6000.0), Arjun (25, 4000.0), Sumit (18, 2000.0)]
```

### Summary

- **Alphabetical Sorting of Strings**: Use `Comparator.naturalOrder()` or `String.CASE_INSENSITIVE_ORDER`.
- **Sorting Integers**: Use `Comparator.naturalOrder()`.
- **Sorting Custom Objects**: Use lambda expressions or method references with `Comparator`.

By leveraging these Java 8 features, you can efficiently sort various types of collections in a clean and readable manner.

Certainly! Here’s a structured guide to some common Java interview questions and answers for both freshers and experienced professionals. These answers are designed to be comprehensive and can be tailored further based on individual experiences.

---

## Java Interview Questions and Answers (Part-1)

### Q1. What are your roles and responsibilities in your projects?

**Answer:**
1. **Requirement Gathering:** Collecting requirements from the functional team to understand the needs and objectives of the project.
2. **Documentation:** Creating and maintaining an Excel sheet or other documentation to log queries related to requirements and tasks.
3. **Meetings:** Attending meetings with the functional team to clarify any queries logged in the query log.
4. **Technical Design Document (TDD):** Preparing the Technical Design Document and obtaining approval from the architect and the functional team.
5. **Code Development:** Writing and developing code based on the approved design.
6. **Unit Testing:** Performing unit testing to ensure the code functions as expected.
7. **Code Review:** Participating in code reviews to ensure code quality and adherence to standards.
8. **Version Control:** Pushing code to the Git repository for version control and collaboration.
9. **Support:** Providing support for System Integration Testing (SIT) and User Acceptance Testing (UAT) environments to ensure the application meets business requirements.

### Q2. Explain your project architecture.

**Answer:**
In my recent project, the architecture is designed for a **Banking System** where users can manage their accounts and perform various financial transactions. Here’s a high-level overview:

1. **User Management:** Handles user registration, authentication, and authorization.
2. **Account Management:** Allows users to open new accounts, modify existing account details, and manage account types (e.g., saving, current).
3. **Transaction Management:** Manages deposit, withdrawal, and transfer operations. Ensures transaction consistency and security.
4. **Loan Management:** Provides functionality for users to apply for loans and check loan eligibility based on their criteria.
5. **Balance Enquiry:** Allows users to check their account balance and transaction history.
6. **Integration Layer:** Interfaces with external systems such as payment gateways and financial services.
7. **Database Layer:** Utilizes relational databases (e.g., MySQL, Oracle) to store user information, transaction details, and account data.
8. **Frontend:** Developed using web technologies (e.g., HTML, CSS, JavaScript, Angular) to provide a user-friendly interface.
9. **Backend:** Built using Java Spring Boot for business logic and REST APIs to interact with the frontend and database.

### Q3. How many modules are there in your project and what are those?

**Answer:**
The project consists of several key modules:

1. **Credential Management:** Handles user authentication, authorization, and security aspects.
2. **Saving Account:** Manages savings accounts including transactions, interest calculations, and account statements.
3. **Current Account:** Deals with current accounts, overdrafts, and associated transaction management.
4. **Deposit Loan:** Manages deposit accounts and loan facilities, including application processing and loan approval.
5. **Balance Enquiry:** Provides functionality for users to view their current account balances and transaction history.
6. **Reporting Module:** Generates reports on account activities, transaction summaries, and financial statements.

### Additional Tips for Interview Preparation:

1. **Understand Your Project:** Be ready to explain not just the architecture but also how each component fits together and the technologies used.
2. **Be Specific:** Tailor your answers to reflect your actual experience and responsibilities.
3. **Prepare Examples:** Use concrete examples from your work to illustrate your role and how you contributed to the project's success.
4. **Stay Updated:** Keep abreast of new developments in Java and related technologies to discuss during interviews.

---

Feel free to adapt and expand upon these answers based on your personal experiences and the specific context of your projects.


Certainly! Here’s a continuation of the Java interview questions and answers with a focus on more advanced or specific topics, especially relevant for those with some experience.

---

## Java Interview Questions and Answers (Part-2)

### Q26. Do you know mocking? Could you please explain a scenario where you have used mocking?

**Answer:**

**Mocking** is a process of creating mock objects to simulate the behavior of real objects in a controlled way. This is particularly useful in unit testing to isolate the component being tested from its dependencies.

**Scenario Example:**

In our application, we had several controller classes that needed to be tested. To write effective unit tests for these controllers, we used mocking to avoid the need for a real database or external service calls. For instance, when testing a controller that interacts with a service layer, we mocked the service layer to return predefined responses. This allowed us to test the controller logic without relying on the actual implementation of the service.

**Implementation Steps:**
1. **Mocking Service Layer:** We used libraries like Mockito to create mock objects of our service classes.
2. **Defining Behavior:** We defined the behavior of these mocks, such as returning specific values when methods are called.
3. **Injecting Mocks:** We injected these mock objects into the controller to test its behavior.
4. **Verifying Interactions:** We verified that the controller interacts with the mock objects as expected.

Mocking was also employed to simulate third-party API calls, ensuring our unit tests did not depend on external systems and could be run in isolation.

### Q27. How do you debug an issue in a production environment?

**Answer:**

**Debugging** in a production environment requires careful handling to avoid impacting the live system. Here’s how I approach it:

1. **Log Analysis:** 
   - **Check Log Files:** Examine application logs for error messages, stack traces, and any unusual patterns. Logs often provide critical information about where the issue occurred.
   - **Use Log Levels:** Ensure different log levels (INFO, DEBUG, ERROR) are properly set up to capture relevant information without overwhelming the log files.

2. **Monitor Performance Metrics:**
   - **Use Monitoring Tools:** Utilize tools like Prometheus, Grafana, or application performance monitoring (APM) solutions to track metrics and identify performance bottlenecks or unusual activity.

3. **Reproduce the Issue:**
   - **Simulate the Environment:** Try to reproduce the issue in a staging environment that mirrors the production setup as closely as possible.
   - **Check Recent Changes:** Investigate recent deployments or configuration changes that might have introduced the issue.

4. **Diagnostic Tools:**
   - **Use Debugging Tools:** Employ tools like Java VisualVM or JConsole to monitor JVM metrics and thread activity.

5. **Hotfixes:**
   - **Deploy Hotfixes:** If a critical issue is identified, deploy a hotfix carefully to address the problem while ensuring minimal impact on users.

### Q28. What is Logging?

**Answer:**

**Logging** is the practice of recording events and activities that occur within an application. It provides a way to track the application's behavior, diagnose issues, and monitor system health. Logs can be used for various purposes, including debugging, auditing, and performance monitoring.

**Types of Logging:**
1. **Information Logging:** Captures general information about application events, such as startup and shutdown sequences.
2. **Error Logging:** Records error messages and exceptions to help diagnose issues.
3. **Debug Logging:** Provides detailed information about the application's internal state, useful during development and troubleshooting.
4. **Audit Logging:** Tracks critical operations for security and compliance purposes.

**Implementation:**
- **Logging Frameworks:** Use frameworks like Log4j, SLF4J, or Java Util Logging to configure and manage logging in your application.
- **Log Levels:** Configure different log levels (e.g., TRACE, DEBUG, INFO, WARN, ERROR) based on the importance and verbosity of the logs.
- **Log Rotation:** Implement log rotation policies to manage log file sizes and archive old logs to prevent disk space issues.

**Example in Code:**
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyClass {
    private static final Logger logger = LoggerFactory.getLogger(MyClass.class);

    public void doSomething() {
        logger.info("Starting the process");
        try {
            // Business logic here
        } catch (Exception e) {
            logger.error("An error occurred", e);
        }
        logger.info("Process completed");
    }
}
```

### Additional Tips:

1. **Be Specific:** Tailor your answers with examples from your own experience to make them more impactful.
2. **Stay Current:** Be aware of recent trends and updates in Java technologies to demonstrate your ongoing learning and adaptation.
3. **Practice:** Regularly practice common interview questions and scenarios to improve your confidence and clarity in responses.

Feel free to expand on these answers with more details based on your experiences and the specific context of the interview.

---

Count Occurrence of each character in String using Java 8

In this tutorial we will learn How to count occurrences of a character from string using java8 stream API.

First we will split string based on “” empty string value and will store into Array of string and after that we will use Arrays.Stream(-) method and convert into stream and after converting into stream we can call .collect(-) method.Inside collect(-) method we can pass the parameter Collectors.groupingBy(-,-) and Inside Collectors.groupingBy(-,-) function we can pass the two parameters one is Function.identity() to find the unique identity of each element and second parameter we can pass Collectors.counting() to count the number of occurrence for each character.

String input = "gainjavaknowledge";

String [] array = input.split("");
 
Map<String, Long> output = Arrays.stream(array).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
import java.util.Arrays;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class CountCharacterOccurrence {

    public static void main(String[] args) {
        String input = "gainjavaknowledge";

        Map<String, Long> output = Arrays.stream(input.split("")).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
        System.out.println("Output : "+output);

    }
}

How to find duplicate elements from string using Java8?
Gain Java Knowledge
Gain Java Knowledge

·
Follow

1 min read
·
Aug 6, 2023
7




In this tutorial, we will learn How to filter out duplicate characters in Java 8.

First we will split string based on “” empty string value and will store into Array of string and after that we will use Arrays.Stream(-) method and convert into stream and after converting into stream we can call .collect(-) method.Inside collect(-) method we can pass the parameter Collectors.groupingBy(-,-) and Inside Collectors.groupingBy(-,-) function we can pass the two parameters one is Function.identity() to find the unique identity of each element and second parameter we can paas Collectors.counting() to count the number of occurrence for each character. After that we again need to convert Map into stream to apply stream functions. So we can call .entrySet().stream() and now we can apply the filter and restrict the data based on condition .filter(ele -> ele.getValue() > 1) . Now we will store all the duplicate keys into ArrayList and print the duplicateElements.

Below is the complete Code :

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class FindDuplicateCharFromString {

    public static void main(String[] args) {

        String input = "gainjavaknowledge";

        List<String> duplicateElements = Arrays.stream(input.split(""))
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                .entrySet().stream()…
}
}


Java Program To Find First Non-Repeated Character In String
Gain Java Knowledge
Gain Java Knowledge

·
Follow

1 min read
·
Aug 13, 2023
64




In this tutorial, we will learn How to find first non repeated characters in Java 8.

First we will split string based on “” empty string value and will store into Array of string and after that we will use Arrays.Stream(-) method and convert into stream and after converting into stream we can call .collect(-) method.Inside collect(-) method we can pass the parameter Collectors.groupingBy(-,-) and Inside Collectors.groupingBy(-,-) function we can pass the two parameters one is Function.identity() to find the unique identity of each element and second parameter we can paas Collectors.counting() to count the number of occurrence for each character. After that we again need to convert Map into stream to apply stream functions. So we can call .entrySet().stream() and now we can apply the filter and restrict the data based on condition .filter(ele -> ele.getValue() == 1) . Now we will store all the non repeated keys into ArrayList and will pick the first non repeated character and Print the output.

Below is the complete Code :

package com.demo.main;

import java.util.Arrays;
import java.util.LinkedHashMap;
import java.util.function.Function;
import java.util.stream.Collectors;

public class FindFirstNonRepeatedCharacter {

    public static void main(String[] args) {

        String input = "gainjavaknowledge";

        String output = Arrays.stream(input.split(""))
                .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new…
}}


| Java 7 | Java 8 |
|---------|---------|
| ```java<br>public class HelloWorld {</br><br>    public static void main(String[] args) {</br><br>        System.out.println("Hello, World!");</br><br>    }</br><br>}</br><br>``` | ```java</br><br>public class HelloWorld {</br><br>    public static void main(String[] args) {</br><br>        System.out.println("Hello, World!");</br><br>    }</br><br>}</br><br>``` |

