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

These examples should help you grasp the concepts and best practices related to Java and Spring Boot. If you need more detailed examples or explanations, feel free to ask!
