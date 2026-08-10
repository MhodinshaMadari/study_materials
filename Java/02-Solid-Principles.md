# SOLID Principles Learning Notes

> Personal Learning Notes
>
> Author: Modinshalli N K
>
> Goal: Intermediate → Advanced Software Design & Architecture

---

# Table of Contents

1. Why SOLID Exists
2. Coupling and Cohesion
3. SRP (Single Responsibility Principle)
4. OCP (Open Closed Principle)
5. LSP (Liskov Substitution Principle)
6. ISP (Interface Segregation Principle)
7. DIP (Dependency Inversion Principle)
8. SOLID Summary
9. Important Interview Notes

---

# Why SOLID Exists

As software grows, code becomes:

- Hard to maintain
- Hard to extend
- Difficult to test
- Risky to modify

Example:

```java
class OrderService {

    public void createOrder() {}

    public void saveOrder() {}

    public void generateInvoice() {}

    public void sendEmail() {}
}
```

Problems:

- Database changes affect class
- Email changes affect class
- Invoice changes affect class

Multiple reasons to change.

SOLID helps build systems that are:

- Maintainable
- Flexible
- Extensible
- Testable

---

# Coupling and Cohesion

Before SOLID, understand these two concepts.

## Coupling

### Definition

Degree of dependency between modules/classes.

### High Coupling (Bad)

```java
class UserService {

    private MySQLDatabase db =
            new MySQLDatabase();
}
```

Problems:

- Tightly bound to MySQL
- Hard to switch databases
- Hard to test

---

### Low Coupling (Good)

```java
interface Database {
    void save();
}
```

```java
class UserService {

    private Database database;

    UserService(Database database) {
        this.database = database;
    }
}
```

Benefits:

- Flexible
- Testable
- Extensible

---

## Cohesion

### Definition

How closely related responsibilities are inside a class.

### Low Cohesion (Bad)

```java
class EmployeeManager {

    void calculateSalary(){}

    void sendEmail(){}

    void exportExcel(){}
}
```

Unrelated responsibilities.

---

### High Cohesion (Good)

```java
class SalaryCalculator {}
```

```java
class EmailService {}
```

```java
class ExportService {}
```

Each class has one focused responsibility.

---

# SRP (Single Responsibility Principle)

## Definition

A class should have only one reason to change.

---

## Bad Example

```java
class EmployeeService {

    void calculateSalary(){}

    void saveEmployee(){}

    void sendWelcomeEmail(){}

    void generateSalaryReport(){}
}
```

Reasons to change:

- Salary rule changes
- Database changes
- Email changes
- Report changes

Multiple responsibilities.

---

## Better Design

```java
class SalaryCalculator {}
```

```java
class EmployeeRepository {}
```

```java
class EmailService {}
```

```java
class ReportService {}
```

---

## Important Note

SRP is NOT:

> One method per class.

SRP IS:

> One business responsibility per class.

---

## Interview Answer

A class should have one reason to change.

The focus is on business responsibility rather than the number of methods.

---

# OCP (Open Closed Principle)

## Definition

Software entities should be:

- Open for Extension
- Closed for Modification

---

## Bad Example

```java
class PaymentService {

    void pay(String type) {

        if(type.equals("UPI")) {}

        if(type.equals("CARD")) {}
    }
}
```

New Requirement:

- Wallet
- PayPal
- NetBanking

Requires modification.

Violation of OCP.

---

## Better Design

### Strategy Interface

```java
interface PaymentStrategy {

    void pay();
}
```

### Implementations

```java
class UpiPayment
        implements PaymentStrategy {
}
```

```java
class CardPayment
        implements PaymentStrategy {
}
```

```java
class WalletPayment
        implements PaymentStrategy {
}
```

To add new payment:

Create a new implementation.

No modification of existing code.

---

## Senior Engineer Tip

Repeated usage of:

```java
if(type.equals(...))
```

or

```java
switch(type)
```

for growing business types usually indicates OCP violation.

---

# LSP (Liskov Substitution Principle)

## Definition

Subtypes must be substitutable for their base types.

---

## Bad Example

```java
interface Bird {

    void fly();
}
```

```java
class Sparrow implements Bird {

    public void fly() {}
}
```

```java
class Ostrich implements Bird {

    public void fly() {
        throw new UnsupportedOperationException();
    }
}
```

Problem:

```java
Bird bird = new Ostrich();

bird.fly();
```

Application crashes.

Contract is broken.

---

## Better Design

```java
interface Bird {

    void eat();
}
```

```java
interface FlyingBird
        extends Bird {

    void fly();
}
```

```java
class Sparrow
        implements FlyingBird {
}
```

```java
class Ostrich
        implements Bird {
}
```

---

## Rectangle-Square Problem

Expected:

```java
Rectangle rect = new Square();

rect.setWidth(5);
rect.setHeight(10);
```

Rectangle expectation:

```text
Area = 50
```

Actual:

```text
Area = 100
```

Behavior changed unexpectedly.

LSP violated.

---

## Interview Answer

LSP is about behavioral contracts.

A subtype must not violate expectations established by the parent abstraction.

---

# ISP (Interface Segregation Principle)

## Definition

Clients should not be forced to depend on methods they do not use.

---

## Bad Example

```java
interface Worker {

    void work();

    void eat();

    void sleep();
}
```

Robot:

```java
class RobotWorker
        implements Worker {

    public void work(){}

    public void eat() {
        throw new UnsupportedOperationException();
    }

    public void sleep() {
        throw new UnsupportedOperationException();
    }
}
```

---

## Better Design

```java
interface Workable {

    void work();
}
```

```java
interface Eatable {

    void eat();
}
```

```java
interface Sleepable {

    void sleep();
}
```

Human:

```java
class HumanWorker
    implements Workable,
               Eatable,
               Sleepable {
}
```

Robot:

```java
class RobotWorker
        implements Workable {
}
```

---

## Vehicle Example

Bad:

```java
interface Vehicle {

    void drive();

    void fly();
}
```

Car:

```java
class Car implements Vehicle {

    public void drive(){}

    public void fly() {
        throw new UnsupportedOperationException();
    }
}
```

Violation:

- ISP
- LSP

---

## Better Design

```java
interface DrivableVehicle {

    void drive();
}
```

```java
interface FlyableVehicle {

    void fly();
}
```

---

# DIP (Dependency Inversion Principle)

## Definition

High-level modules should not depend on low-level modules.

Both should depend on abstractions.

---

## Bad Example

```java
class UserService {

    private MySQLDatabase db =
            new MySQLDatabase();
}
```

Tightly coupled.

---

## Better Design

```java
interface Database {

    void save();
}
```

```java
class MySQLDatabase
        implements Database {
}
```

```java
class MongoDatabase
        implements Database {
}
```

```java
class UserService {

    private Database database;

    public UserService(Database database) {
        this.database = database;
    }
}
```

Benefits:

- Loose coupling
- Easy testing
- Extensibility
- Better maintainability

---

## DIP vs DI

### DIP

Design Principle

```java
UserService -> Database
```

Depend on abstraction.

---

### DI

Implementation technique.

Examples:

```java
Constructor Injection
```

```java
@Autowired
```

Spring Dependency Injection.

---

# SOLID Summary

| Principle | Meaning |
|------------|------------|
| SRP | One reason to change |
| OCP | Open for extension, closed for modification |
| LSP | Child should honor parent's contract |
| ISP | Small focused interfaces |
| DIP | Depend on abstractions |

---

# Easy Memory Trick

S → Single Responsibility

O → Open for Extension

L → Liskov Substitution

I → Interface Segregation

D → Dependency Inversion

SOLID

---

# Common Interview Questions

### Q1

What is the difference between DIP and DI?

Answer:

- DIP is a design principle.
- DI is a technique used to implement DIP.

---

### Q2

Which SOLID Principle leads naturally to Strategy Pattern?

Answer:

OCP

---

### Q3

What is the main symptom of ISP violation?

Answer:

Frequent usage of:

```java
throw new UnsupportedOperationException();
```

---

### Q4

What is LSP primarily about?

Answer:

Behavioral contracts and substitutability.

---

### Q5

What is the ultimate design goal behind SOLID?

Answer:

- High Cohesion
- Low Coupling
- Maintainable Code
- Extensible Systems

---
