### 🧠 What is Dependency Injection (DI)?

**Dependency Injection (DI)** is a design pattern used to implement **Inversion of Control (IoC)**, allowing objects to receive their dependencies from an external source rather than creating them internally. This promotes loose coupling, easier testing, and better maintainability.

---

### 🔧 Why Use DI?

- **Decouples components**: Classes don’t need to know how their dependencies are created.
- **Improves testability**: You can easily mock dependencies.
- **Simplifies configuration**: Spring handles object creation and wiring.

---

### 📦 Types of Dependency Injection in Spring

1. **Constructor Injection**
2. **Setter Injection**
3. **Field Injection** (not recommended for testing)

---

### ✅ Example: Constructor Injection in Spring Boot

Let’s walk through a simple example:

#### 📁 Service Class
```java
@Service
public class GreetingService {
    public String greet() {
        return "Hello, Spring Boot!";
    }
}
```

#### 📁 Controller Class
```java
@RestController
public class GreetingController {

    private final GreetingService greetingService;

    // Constructor Injection
    public GreetingController(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    @GetMapping("/greet")
    public String greet() {
        return greetingService.greet();
    }
}
```

### 🔍 What’s Happening Here?

- Spring Boot automatically detects `@Service` and creates a `GreetingService` bean.
- It injects this bean into `GreetingController` via the constructor.
- When you hit `/greet`, the controller uses the injected service to return a greeting.

---

### 🧪 Benefits in Practice

- **Unit Testing** becomes easy:
  ```java
  GreetingService mockService = mock(GreetingService.class);
  GreetingController controller = new GreetingController(mockService);
  ```

- **Loose Coupling**: You can swap `GreetingService` with another implementation without changing the controller.

---
### 🧠 What is Setter Injection in Spring?

**Setter Injection** is a form of **Dependency Injection (DI)** where Spring injects dependencies into a bean using **setter methods**. This approach allows you to set dependencies after the object is constructed, offering flexibility in configuration.

---

### 🔧 How Setter Injection Works

Spring:
1. Creates the bean using a no-argument constructor.
2. Calls the setter method to inject the dependency.

---

### ✅ Example: Setter Injection in Spring Boot

#### 📁 Service Class
```java
@Service
public class GreetingService {
    public String greet() {
        return "Hello from Setter Injection!";
    }
}
```

#### 📁 Controller Class Using Setter Injection
```java
@RestController
public class GreetingController {

    private GreetingService greetingService;

    // Setter method for DI
    @Autowired
    public void setGreetingService(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    @GetMapping("/greet")
    public String greet() {
        return greetingService.greet();
    }
}
```

---

### 🔍 Key Points

- The `@Autowired` annotation tells Spring to inject the dependency.
- Setter Injection is useful when:
  - You want optional dependencies.
  - You need to change dependencies after object creation.
- Spring can also perform setter injection without `@Autowired` if there's only one bean of the required type.

---

### ⚖️ Comparison with Constructor Injection

| Feature               | Constructor Injection         | Setter Injection              |
|-----------------------|-------------------------------|-------------------------------|
| Injection Time        | At object creation            | After object creation         |
| Required Dependencies | Enforced                      | Optional                      |
| Testability           | Easier with final fields      | Slightly harder               |
| Flexibility           | Less flexible                 | More flexible                 |

---
### 🧠 What is Field Injection in Spring?

**Field Injection** is a form of Dependency Injection where Spring injects dependencies directly into the fields of a class using the `@Autowired` annotation. It’s the most concise form of DI but comes with trade-offs, especially in terms of **testability and maintainability**.

---

### ✅ Example: Field Injection in Spring Boot

#### 📁 Service Class
```java
@Service
public class GreetingService {
    public String greet() {
        return "Hello from Field Injection!";
    }
}
```

#### 📁 Controller Class Using Field Injection
```java
@RestController
public class GreetingController {

    @Autowired
    private GreetingService greetingService;

    @GetMapping("/greet")
    public String greet() {
        return greetingService.greet();
    }
}
```

---

### 🔍 How It Works

- Spring scans the class and sees the `@Autowired` annotation.
- It injects the `GreetingService` bean directly into the field.
- No constructor or setter method is needed.

---

### ⚠️ Why Field Injection Is Not Recommended (Especially for Testing)

| Concern               | Explanation |
|-----------------------|-------------|
| **Testability**       | You can't easily inject mock dependencies without using reflection or Spring context. |
| **Immutability**      | Fields are not `final`, making the class mutable and less robust. |
| **Encapsulation**     | Breaks encapsulation by exposing internal structure to the framework. |
| **Hidden Dependencies** | Dependencies are not visible in constructors, making code harder to understand and maintain. |

---

### ✅ Better Alternatives

- **Constructor Injection** is preferred:
  - Makes dependencies explicit.
  - Supports immutability.
  - Easier to test with plain Java objects.

---

### 🧪 Testing Challenge Example

With field injection:
```java
GreetingController controller = new GreetingController(); // No way to inject mock GreetingService easily
```

With constructor injection:
```java
GreetingService mockService = mock(GreetingService.class);
GreetingController controller = new GreetingController(mockService); // Easy to test
```

---

Would you like a **visual comparison** of all three DI types or a **code template** showing how to refactor field injection to constructor injection?
