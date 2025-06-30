An **Entity Class** is a fundamental concept in object-oriented programming (OOP), especially in the context of software design and development using languages like Java, C#, or Python. It represents a **real-world object** or **concept** in a software application and is typically used to model data.

---

### 🔹 What is an Entity Class?

An **Entity Class**:
- Represents a **single table** in a database (in the context of ORM – Object-Relational Mapping).
- Each **instance** of the class corresponds to a **row** in the table.
- Contains **fields** (or properties) that map to **columns** in the table.
- Often includes **getters and setters**, **constructors**, and sometimes **annotations** (in Java or C#) to define how it maps to the database.

---

### 🔸 Example in Java (Using JPA - Java Persistence API)

```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "employees")
public class Employee {

    @Id
    private int id;
    private String name;
    private String department;
    private double salary;

    // Constructors
    public Employee() {}

    public Employee(int id, String name, String department, double salary) {
        this.id = id;
        this.name = name;
        this.department = department;
        this.salary = salary;
    }

    // Getters and Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getDepartment() { return department; }
    public void setDepartment(String department) { this.department = department; }

    public double getSalary() { return salary; }
    public void setSalary(double salary) { this.salary = salary; }
}
```

---

### 🔸 Explanation

- `@Entity`: Marks the class as a JPA entity.
- `@Table(name = "employees")`: Maps the class to the `employees` table in the database.
- `@Id`: Marks the `id` field as the primary key.
- Fields like `name`, `department`, and `salary` represent columns in the table.
- The class includes constructors and getter/setter methods for encapsulation.

---

### 🔹 Real-World Analogy

Think of an **Entity Class** like a **blueprint** for a real-world object. For example:
- A `Car` entity class might have fields like `make`, `model`, `year`, and `price`.
- Each car in a dealership's inventory is an **instance** of the `Car` class.

---
In **Spring Boot with Spring Data JPA**, **associations** between entity classes represent relationships between tables in a relational database. These associations are modeled using annotations like `@OneToOne`, `@OneToMany`, `@ManyToOne`, and `@ManyToMany`.

Let’s go through each type of association with a practical example.

---

## 🔹 1. `@OneToOne` Association

### Example: Each `Employee` has one `Address`.

```java
@Entity
public class Employee {
    @Id
    private int id;
    private String name;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id")
    private Address address;
}
```

```java
@Entity
public class Address {
    @Id
    private int id;
    private String city;
    private String state;
}
```

---

## 🔹 2. `@OneToMany` and `@ManyToOne` Association

### Example: One `Department` has many `Employees`.

```java
@Entity
public class Department {
    @Id
    private int id;
    private String deptName;

    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
    private List<Employee> employees;
}
```

```java
@Entity
public class Employee {
    @Id
    private int id;
    private String name;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;
}
```

---

## 🔹 3. `@ManyToMany` Association

### Example: An `Employee` can work on many `Projects`, and a `Project` can have many `Employees`.

```java
@Entity
public class Employee {
    @Id
    private int id;
    private String name;

    @ManyToMany
    @JoinTable(
        name = "employee_project",
        joinColumns = @JoinColumn(name = "employee_id"),
        inverseJoinColumns = @JoinColumn(name = "project_id")
    )
    private List<Project> projects;
}
```

```java
@Entity
public class Project {
    @Id
    private int id;
    private String projectName;

    @ManyToMany(mappedBy = "projects")
    private List<Employee> employees;
}
```

---

## 🔸 Repository Example

```java
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
    List<Employee> findByDepartmentDeptName(String deptName);
}
```

---

## 🔸 Service Layer Example

```java
@Service
public class EmployeeService {
    @Autowired
    private EmployeeRepository employeeRepository;

    public List<Employee> getEmployeesByDepartment(String deptName) {
        return employeeRepository.findByDepartmentDeptName(deptName);
    }
}
```

---
Let’s break down the key attributes of the `@OneToMany` annotation in Spring Data JPA, especially focusing on:

- `mappedBy`
- `cascade`
- Other commonly used attributes like `fetch` and `orphanRemoval`

---

## 🔹 1. `mappedBy = "department"`

### ✅ Purpose:
- Indicates that the **ownership of the relationship** is on the **other side** (i.e., the `Employee` entity).
- Prevents the creation of an unnecessary **join table**.

### ✅ Example:
```java
@OneToMany(mappedBy = "department")
private List<Employee> employees;
```

In the `Employee` entity:
```java
@ManyToOne
@JoinColumn(name = "department_id")
private Department department;
```

### ✅ Meaning:
- The `Employee` entity owns the relationship.
- The `Department` entity is the **inverse side** and uses `mappedBy` to refer to the owning field.

---

## 🔹 2. `cascade = CascadeType.ALL`

### ✅ Purpose:
- Specifies that **persistence operations** (like `persist`, `merge`, `remove`, etc.) should **cascade** from the parent (`Department`) to the child (`Employee`).

### ✅ Cascade Types:
| Cascade Type       | Description |
|--------------------|-------------|
| `PERSIST`          | Saves child entities when the parent is saved. |
| `MERGE`            | Updates child entities when the parent is updated. |
| `REMOVE`           | Deletes child entities when the parent is deleted. |
| `REFRESH`          | Refreshes child entities when the parent is refreshed. |
| `DETACH`           | Detaches child entities when the parent is detached. |
| `ALL`              | Applies all of the above. |

### ✅ Example:
```java
@OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
private List<Employee> employees;
```

If you save a `Department` with a list of `Employee` objects, all employees will be saved automatically.

---

## 🔹 3. `fetch = FetchType.LAZY` (or `EAGER`)

### ✅ Purpose:
- Controls **when** the related entities are loaded from the database.

| Fetch Type | Description |
|------------|-------------|
| `LAZY`     | Loads the related entities **on demand** (default for `@OneToMany`). |
| `EAGER`    | Loads the related entities **immediately** with the parent. |

### ✅ Example:
```java
@OneToMany(mappedBy = "department", fetch = FetchType.LAZY)
private List<Employee> employees;
```

---

## 🔹 4. `orphanRemoval = true`

### ✅ Purpose:
- Automatically deletes child entities that are **removed from the parent’s collection**.

### ✅ Example:
```java
@OneToMany(mappedBy = "department", cascade = CascadeType.ALL, orphanRemoval = true)
private List<Employee> employees;
```

If you remove an `Employee` from the `employees` list of a `Department`, that `Employee` will be deleted from the database.

---

## 🔸 Summary Table

| Attribute        | Purpose |
|------------------|---------|
| `mappedBy`       | Defines the inverse side of the relationship. |
| `cascade`        | Propagates operations from parent to child. |
| `fetch`          | Controls loading strategy (lazy vs eager). |
| `orphanRemoval`  | Deletes child entities removed from the collection. |

---
