# Design Patterns - Learning Notes

> Author: Modinshalli N K
>
> Goal: Intermediate → Advanced Software Design
>
> Language: Java / Spring Boot

---

# Table of Contents

1. Strategy Pattern
2. Factory Pattern
3. Builder Pattern
4. Observer Pattern
5. Decorator Pattern
6. Pattern Comparison
7. Pattern Selection Cheat Sheet
8. Real World Architecture Example

---

# Strategy Pattern

## Intent

Encapsulate interchangeable algorithms and allow them to be selected at runtime.

---

## Problem

Avoid growing if-else ladders.

Bad:

```java
class PaymentService {

    public void pay(String type) {

        if(type.equals("UPI")) {
            System.out.println("UPI Payment");
        }

        if(type.equals("CARD")) {
            System.out.println("Card Payment");
        }

        if(type.equals("PAYPAL")) {
            System.out.println("PayPal Payment");
        }
    }
}
```

Problems:

- OCP violation
- Difficult to maintain
- Difficult to test

---

## Solution

### Strategy Interface

```java
public interface PaymentStrategy {
    void pay(double amount);
}
```

---

### Concrete Strategies

```java
public class UpiPayment
        implements PaymentStrategy {

    @Override
    public void pay(double amount) {

        System.out.println(
            "UPI Payment : " + amount
        );
    }
}
```

```java
public class CardPayment
        implements PaymentStrategy {

    @Override
    public void pay(double amount) {

        System.out.println(
            "Card Payment : " + amount
        );
    }
}
```

---

### Context

```java
public class PaymentService {

    private final PaymentStrategy strategy;

    public PaymentService(
            PaymentStrategy strategy) {

        this.strategy = strategy;
    }

    public void processPayment(
            double amount) {

        strategy.pay(amount);
    }
}
```

---

### Usage

```java
PaymentStrategy strategy =
        new UpiPayment();

PaymentService service =
        new PaymentService(strategy);

service.processPayment(500);
```

---

## Real Use Cases

- Payment Methods
- Discount Engine
- Notification Channels
- Authentication Providers
- Compression Algorithms

---

# Factory Pattern

## Intent

Centralize object creation logic.

---

## Problem

Object creation is scattered everywhere.

Bad:

```java
if(type.equals("PDF"))
    return new PdfReport();

if(type.equals("EXCEL"))
    return new ExcelReport();
```

---

## Solution

### Product Interface

```java
public interface Report {

    void generate();
}
```

---

### Concrete Products

```java
public class PdfReport
        implements Report {

    @Override
    public void generate() {

        System.out.println(
            "Generating PDF"
        );
    }
}
```

```java
public class ExcelReport
        implements Report {

    @Override
    public void generate() {

        System.out.println(
            "Generating Excel"
        );
    }
}
```

---

### Factory

```java
public class ReportFactory {

    public Report getReport(
            String type) {

        switch(type) {

            case "PDF":
                return new PdfReport();

            case "EXCEL":
                return new ExcelReport();

            default:
                throw new IllegalArgumentException(
                    "Unsupported Report Type"
                );
        }
    }
}
```

---

### Usage

```java
ReportFactory factory =
        new ReportFactory();

Report report =
        factory.getReport("PDF");

report.generate();
```

---

## Real Use Cases

- Report Generation
- Payment Gateway Creation
- Notification Channel Selection
- Database Provider Selection

---

# Builder Pattern

## Intent

Create complex objects step-by-step.

---

## Problem

Constructor explosion.

Bad:

```java
new Employee(
    "ID001",
    "Modinshalli",
    "test@mail.com",
    "Engineering",
    "Senior Engineer",
    1500000,
    "Manager"
);
```

---

## Solution

### Entity

```java
public class Employee {

    private String id;
    private String name;
    private String email;

    private Employee(
            EmployeeBuilder builder) {

        this.id = builder.id;
        this.name = builder.name;
        this.email = builder.email;
    }

    public static class EmployeeBuilder {

        private String id;
        private String name;
        private String email;

        public EmployeeBuilder id(
                String id) {

            this.id = id;
            return this;
        }

        public EmployeeBuilder name(
                String name) {

            this.name = name;
            return this;
        }

        public EmployeeBuilder email(
                String email) {

            this.email = email;
            return this;
        }

        public Employee build() {

            return new Employee(this);
        }
    }
}
```

---

### Usage

```java
Employee employee =
    new Employee.EmployeeBuilder()
        .id("EMP001")
        .name("Modinshalli")
        .email("modi@test.com")
        .build();
```

---

## Advantages

- Readable
- Flexible
- Supports immutability
- Avoids telescoping constructors

---

## Real Use Cases

- DTOs
- Request Objects
- Configuration Objects
- Lombok @Builder

---

# Observer Pattern

## Intent

Notify multiple interested objects when an event occurs.

---

## Real World Example

Order Created Event

Needs:

- Email Notification
- Inventory Update
- Billing
- Audit Logging

---

## Observer Interface

```java
public interface OrderObserver {

    void onOrderCreated(
            Order order);
}
```

---

## Concrete Observer

```java
public class EmailService
        implements OrderObserver {

    @Override
    public void onOrderCreated(
            Order order) {

        System.out.println(
                "Email Sent"
        );
    }
}
```

---

## Subject

```java
public class OrderPublisher {

    private List<OrderObserver>
            observers =
            new ArrayList<>();

    public void register(
            OrderObserver observer) {

        observers.add(observer);
    }

    public void notifyObservers(
            Order order) {

        observers.forEach(
                observer ->
                        observer.onOrderCreated(order)
        );
    }
}
```

---

## Usage

```java
publisher.register(
        new EmailService());

publisher.register(
        new InventoryService());

publisher.notifyObservers(order);
```

---

## Spring Boot Equivalent

```java
publisher.publishEvent(
        new OrderCreatedEvent(order));
```

Listeners:

```java
@EventListener
public void handle(
        OrderCreatedEvent event) {
}
```

---

## Real Use Cases

- Spring Events
- Kafka Consumers
- RabbitMQ
- Audit Logs
- Notifications

---

# Decorator Pattern

## Intent

Add behavior dynamically without modifying existing classes.

---

## Problem

Class explosion.

Bad:

```java
EmailService

EmailServiceWithLogging

EmailServiceWithRetry

EmailServiceWithMetrics

EmailServiceWithRetryAndMetrics
```

---

## Solution

### Component

```java
public interface NotificationService {

    void send(String message);
}
```

---

### Concrete Component

```java
public class EmailService
        implements NotificationService {

    @Override
    public void send(String message) {

        System.out.println(
                "Sending Email"
        );
    }
}
```

---

### Base Decorator

```java
public abstract class NotificationDecorator
        implements NotificationService {

    protected NotificationService service;

    public NotificationDecorator(
            NotificationService service) {

        this.service = service;
    }
}
```

---

### Logging Decorator

```java
public class LoggingDecorator
        extends NotificationDecorator {

    public LoggingDecorator(
            NotificationService service) {

        super(service);
    }

    @Override
    public void send(String message) {

        System.out.println(
                "Logging Request"
        );

        service.send(message);
    }
}
```

---

### Retry Decorator

```java
public class RetryDecorator
        extends NotificationDecorator {

    public RetryDecorator(
            NotificationService service) {

        super(service);
    }

    @Override
    public void send(String message) {

        System.out.println(
                "Retry Logic"
        );

        service.send(message);
    }
}
```

---

### Usage

```java
NotificationService service =
        new EmailService();

service =
        new LoggingDecorator(service);

service =
        new RetryDecorator(service);

service.send("Hello");
```

---

### Output

```text
Retry Logic
Logging Request
Sending Email
```

---

## Real Use Cases

- Logging
- Retry
- Metrics
- Auditing
- Java IO Streams
- Spring Security Filters

---

# Pattern Comparison

| Pattern | Purpose |
|----------|----------|
| Strategy | Choose behavior at runtime |
| Factory | Create object |
| Builder | Construct complex object |
| Observer | Notify multiple listeners |
| Decorator | Add behavior dynamically |

---

# Pattern Selection Cheat Sheet

## Growing if-else logic

Use:

```text
Strategy Pattern
```

---

## Scattered object creation

Use:

```text
Factory Pattern
```

---

## Complex object construction

Use:

```text
Builder Pattern
```

---

## One event, many listeners

Use:

```text
Observer Pattern
```

---

## Add functionality without modifying source

Use:

```text
Decorator Pattern
```

---

# Real World Notification Platform

Requirements:

- Email
- SMS
- WhatsApp

Future:

- Telegram
- Slack

Extra Features:

- Retry
- Logging
- Metrics

Audit Trail:

- Notification History

---

## Patterns Used

### Strategy

Choose notification channel.

```java
NotificationStrategy
```

---

### Factory

Create notification strategy.

```java
NotificationFactory
```

---

### Decorator

Add:

- Logging
- Retry
- Metrics

---

### Observer

Notify:

- Audit Service
- Analytics Service
- Monitoring Service

---

## Architecture

```text
Request
   |
Factory
   |
Strategy
   |
Decorators
   |
Notification Sent
   |
Observers
```

---

# Interview Summary

### Strategy

Which algorithm should execute?

### Factory

Which object should be created?

### Builder

How should this object be constructed?

### Observer

Who should be notified of this event?

### Decorator

How can I add behavior without changing existing code?

---
