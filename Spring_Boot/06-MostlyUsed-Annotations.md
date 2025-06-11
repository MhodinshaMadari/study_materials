Here’s a list of the **most commonly used and essential annotations in Spring Boot and Spring MVC**, along with explanations and examples to help you understand when and how to use them:

---

## 🔹 Core Spring Annotations

### 1. `@SpringBootApplication`
- **Purpose**: Combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`.
- **Use**: Main entry point of a Spring Boot application.

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

---

### 2. `@Component`, `@Service`, `@Repository`, `@Controller`
- **Purpose**: Marks a class as a Spring-managed component.
- **Differences**:
  - `@Component`: Generic stereotype.
  - `@Service`: Business logic layer.
  - `@Repository`: Data access layer.
  - `@Controller`: Web controller.

```java
@Service
public class UserService {
    public String getUser() {
        return "John Doe";
    }
}
```

---

### 3. `@Autowired`
- **Purpose**: Injects dependencies automatically.
- **Use**: Constructor, field, or setter injection.

```java
@RestController
public class UserController {

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```

---

## 🔹 Web Layer Annotations

### 4. `@RestController`
- **Purpose**: Combines `@Controller` and `@ResponseBody`.
- **Use**: For REST APIs.

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }
}
```

---

### 5. `@RequestMapping`, `@GetMapping`, `@PostMapping`, etc.
- **Purpose**: Maps HTTP requests to handler methods.
- **Use**: Define endpoints.

```java
@GetMapping("/users")
public List<User> getUsers() {
    return userService.getAllUsers();
}
```

---

### 6. `@PathVariable` and `@RequestParam`
- **Purpose**: Extract values from the URL or query parameters.

```java
@GetMapping("/users/{id}")
public User getUserById(@PathVariable Long id) {
    return userService.findById(id);
}

@GetMapping("/search")
public List<User> search(@RequestParam String name) {
    return userService.searchByName(name);
}
```

---

## 🔹 Configuration and Bean Management

### 7. `@Configuration` and `@Bean`
- **Purpose**: Define beans manually in Java config.

```java
@Configuration
public class AppConfig {
    @Bean
    public ModelMapper modelMapper() {
        return new ModelMapper();
    }
}
```

---

### 8. `@Value`
- **Purpose**: Inject values from `application.properties`.

```java
@Value("${app.name}")
private String appName;
```

---

## 🔹 JPA and Database

### 9. `@Entity`, `@Id`, `@GeneratedValue`
- **Purpose**: Define JPA entities.

```java
@Entity
public class User {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
}
```

---

### 10. `@Repository`
- **Purpose**: Marks a class as a DAO and enables exception translation.

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {}
```

---
Here’s your **Spring Boot & Spring MVC Annotations Cheat Sheet** — a printable image summarizing the most commonly used annotations, their purpose, and example usage:

![Spring Boot & Spring MVC Annotations Cheat Sheet](blob:https://m365.cloud.microsoft/b1ab5900-3557-479a-8099-ffe83168cfca)

---
Here are some **advanced annotations** in Spring Boot and Spring Framework that are especially useful for more complex or fine-tuned applications:

---

## 🔧 **Advanced Spring Annotations**

### 1. `@Conditional`
- **Purpose**: Load a bean conditionally based on custom logic.
- **Use Case**: Enable beans only in certain environments or configurations.

```java
@Conditional(MyCustomCondition.class)
@Bean
public MyService myService() {
    return new MyService();
}
```

---

### 2. `@Profile`
- **Purpose**: Activate beans only for specific profiles (e.g., `dev`, `prod`).
- **Use Case**: Environment-specific configurations.

```java
@Profile("dev")
@Bean
public DataSource devDataSource() {
    return new H2DataSource();
}
```

---

### 3. `@Async`
- **Purpose**: Run methods asynchronously in a separate thread.
- **Use Case**: Background tasks, email sending, etc.

```java
@Async
public void sendEmail(String to) {
    // send email logic
}
```

> Requires `@EnableAsync` in a configuration class.

---

### 4. `@Scheduled`
- **Purpose**: Schedule tasks to run at fixed intervals or cron expressions.
- **Use Case**: Periodic jobs like cleanup, reporting, etc.

```java
@Scheduled(cron = "0 0 * * * ?")
public void hourlyTask() {
    // task logic
}
```

> Requires `@EnableScheduling`.

---

### 5. `@EventListener`
- **Purpose**: Listen for application events.
- **Use Case**: Decoupled event-driven architecture.

```java
@EventListener
public void handleUserCreated(UserCreatedEvent event) {
    // handle event
}
```

---

### 6. `@Transactional`
- **Purpose**: Manage transactions declaratively.
- **Use Case**: Rollback on exceptions, commit on success.

```java
@Transactional
public void saveUser(User user) {
    userRepository.save(user);
}
```

---

### 7. `@Import`
- **Purpose**: Import additional configuration classes.
- **Use Case**: Modular configuration.

```java
@Import({SecurityConfig.class, DataSourceConfig.class})
public class AppConfig {}
```

---

### 8. `@Lookup`
- **Purpose**: Inject prototype-scoped beans into singleton beans.
- **Use Case**: Dynamic bean retrieval.

```java
@Lookup
public MyPrototypeBean getPrototypeBean() {
    return null; // Spring overrides this method
}
```

---

Here’s a list of **additional useful Spring annotations** that are often overlooked but can significantly enhance your application's flexibility, modularity, and maintainability:

---

## 🧩 **Other Useful Spring Annotations**

### 1. `@EnableConfigurationProperties`
- **Purpose**: Binds external configuration properties to Java classes.
- **Use Case**: Clean separation of config values.

```java
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private int timeout;
}
```

```java
@EnableConfigurationProperties(AppProperties.class)
@Configuration
public class AppConfig {}
```

---

### 2. `@ResponseStatus`
- **Purpose**: Set HTTP status code for a controller method or exception.
- **Use Case**: Custom error handling.

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {}
```

---

### 3. `@CrossOrigin`
- **Purpose**: Enables CORS for REST APIs.
- **Use Case**: Allow frontend apps from different domains to access APIs.

```java
@CrossOrigin(origins = "http://localhost:3000")
@GetMapping("/users")
public List<User> getUsers() {
    return userService.getAllUsers();
}
```

---

### 4. `@RestControllerAdvice`
- **Purpose**: Global exception handling for REST controllers.
- **Use Case**: Centralized error responses.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
    }
}
```

---

### 5. `@EnableWebMvc`
- **Purpose**: Enables full Spring MVC configuration.
- **Use Case**: When you want to customize Spring MVC behavior.

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {}
```

---

### 6. `@Primary`
- **Purpose**: Marks a bean as the default when multiple candidates exist.
- **Use Case**: Avoid ambiguity in dependency injection.

```java
@Bean
@Primary
public DataSource defaultDataSource() {
    return new HikariDataSource();
}
```

---

### 7. `@Qualifier`
- **Purpose**: Specifies which bean to inject when multiple candidates exist.
- **Use Case**: Fine-grained control over bean selection.

```java
@Autowired
@Qualifier("customDataSource")
private DataSource dataSource;
```

---

### 8. `@Order`
- **Purpose**: Defines the order of execution for components.
- **Use Case**: Filters, interceptors, or event listeners.

```java
@Order(1)
@Component
public class FirstFilter implements Filter {}
```

---
### `Hibernate Annotation`
Here’s a **detailed explanation** of the most commonly used **Hibernate annotations** in a Spring application, along with **clear examples** to help you understand how and when to use them:

---

## 🗂️ Entity and Table Mapping

### 1. `@Entity`
Marks a class as a JPA entity (i.e., a table in the database).

```java
@Entity
public class User {
    @Id
    private Long id;
    private String name;
}
```

### 2. `@Table`
Specifies the table name and schema (optional).

```java
@Entity
@Table(name = "users", schema = "public")
public class User {
    @Id
    private Long id;
}
```

---

## 🔑 Primary Key and Generation

### 3. `@Id`
Defines the primary key of the entity.

```java
@Id
private Long id;
```

### 4. `@GeneratedValue`
Specifies how the primary key is generated.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

- `IDENTITY`: Auto-increment (MySQL)
- `SEQUENCE`: Uses a database sequence (PostgreSQL)
- `AUTO`: Let JPA choose
- `TABLE`: Uses a table to simulate a sequence

### 5. `@SequenceGenerator`
Used with `SEQUENCE` strategy.

```java
@SequenceGenerator(name = "user_seq", sequenceName = "user_sequence", allocationSize = 1)
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq")
```

---

## 📦 Column Mapping

### 6. `@Column`
Customizes column mapping.

```java
@Column(name = "email", nullable = false, unique = true)
private String email;
```

### 7. `@Transient`
Excludes a field from persistence.

```java
@Transient
private String tempPassword;
```

---

## 🔗 Relationships

### 8. `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`

```java
@OneToMany(mappedBy = "user")
private List<Order> orders;

@ManyToOne
@JoinColumn(name = "user_id")
private User user;
```

### 9. `@JoinColumn`
Defines the foreign key column.

```java
@JoinColumn(name = "user_id")
private User user;
```

### 10. `@JoinTable`
Used in many-to-many relationships.

```java
@ManyToMany
@JoinTable(
    name = "user_roles",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "role_id")
)
private Set<Role> roles;
```

---

## 🕒 Timestamps and Versioning

### 11. `@Temporal`
Specifies the temporal type for `Date`.

```java
@Temporal(TemporalType.TIMESTAMP)
private Date createdAt;
```

### 12. `@Version`
Used for optimistic locking.

```java
@Version
private int version;
```

---

## 🧠 Advanced Hibernate-Specific

### 13. `@Type`
Custom Hibernate type mapping.

```java
@Type(type = "json")
@Column(columnDefinition = "json")
private String metadata;
```

### 14. `@Formula`
Maps a field to a SQL expression.

```java
@Formula("(select count(*) from orders o where o.user_id = id)")
private int orderCount;
```

---
