### 🧬 JPA Inheritance Mapping — Detailed Explanation with Example

In **JPA**, **inheritance mapping** allows you to map a Java class hierarchy to database tables. This is useful when you have a base class and multiple subclasses that share common fields but also have their own specific fields.

---

## 🧱 Why Use Inheritance Mapping?

- Promotes **code reuse** and **clean design**
- Models **real-world hierarchies** (e.g., `Employee` → `Manager`, `Developer`)
- Reduces duplication in entity definitions

---

## 🧩 Strategies for Inheritance Mapping

JPA provides **three strategies** for mapping inheritance:

| Strategy | Description | Pros | Cons |
|----------|-------------|------|------|
| `SINGLE_TABLE` | All classes in one table | Fast queries, simple schema | Wasted space (nullable columns) |
| `JOINED` | One table per class, joined by foreign keys | Normalized, efficient storage | Slower joins |
| `TABLE_PER_CLASS` | One table per concrete class | No joins, fast inserts | No shared constraints, complex queries |

---

## ✅ Example: `Employee` Hierarchy

### 1. **Base Class: `Employee`**
```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

### 2. **Subclass: `Manager`**
```java
@Entity
public class Manager extends Employee {
    private String department;
}
```

### 3. **Subclass: `Developer`**
```java
@Entity
public class Developer extends Employee {
    private String programmingLanguage;
}
```

---

## 🗃️ Database Structure (JOINED Strategy)

| employee (base table) |
|-----------------------|
| id (PK)               |
| name                  |

| manager (subclass table) |
|--------------------------|
| id (PK, FK to employee)  |
| department               |

| developer (subclass table) |
|----------------------------|
| id (PK, FK to employee)    |
| programming_language       |

---

## 📦 Repository Example

```java
public interface EmployeeRepository extends JpaRepository<Employee, Long> {}
```

You can also create separate repositories for `Manager` and `Developer` if needed.

---

## 🧪 Querying

```java
List<Employee> allEmployees = employeeRepository.findAll();
```

JPA will automatically fetch the correct subclass (`Manager` or `Developer`) based on the discriminator.

---

## 🧠 Tips

- Use `@DiscriminatorColumn` and `@DiscriminatorValue` for `SINGLE_TABLE` strategy.
- Use `abstract` base classes when they’re not meant to be instantiated.
- Choose strategy based on **query performance**, **data volume**, and **schema design**.

---
Here’s a **detailed explanation** of the three JPA inheritance mapping strategies — `SINGLE_TABLE`, `JOINED`, and `TABLE_PER_CLASS` — including when to use them, how they work, and how to implement them with annotations like `@DiscriminatorColumn` and `@DiscriminatorValue`.

---

## 🧬 1. `SINGLE_TABLE` Strategy

### ✅ Description:
- All classes in the hierarchy are mapped to **a single database table**.
- A **discriminator column** is used to distinguish between entity types.

### 📦 Pros:
- Fastest performance (no joins).
- Simplest schema.

### ⚠️ Cons:
- Wasted space due to many nullable columns.
- Can become hard to manage with large hierarchies.

### 🧪 Example:

#### Base Class
```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "employee_type", discriminatorType = DiscriminatorType.STRING)
public abstract class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```

#### Subclasses
```java
@Entity
@DiscriminatorValue("MANAGER")
public class Manager extends Employee {
    private String department;
}

@Entity
@DiscriminatorValue("DEVELOPER")
public class Developer extends Employee {
    private String programmingLanguage;
}
```

#### Resulting Table: `employee`

| id | name   | department | programming_language | employee_type |
|----|--------|------------|----------------------|----------------|
| 1  | Alice  | HR         | NULL                 | MANAGER        |
| 2  | Bob    | NULL       | Java                 | DEVELOPER      |

---

## 🧱 2. `JOINED` Strategy

### ✅ Description:
- Each class has its own table.
- Subclass tables are **joined** with the base table using foreign keys.

### 📦 Pros:
- Normalized schema.
- Efficient storage (no nulls).

### ⚠️ Cons:
- Slower queries due to joins.
- More complex schema.

### 🧪 Example:

#### Base Class
```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```

#### Subclasses
```java
@Entity
public class Manager extends Employee {
    private String department;
}

@Entity
public class Developer extends Employee {
    private String programmingLanguage;
}
```

#### Resulting Tables:

- `employee`: `id`, `name`
- `manager`: `id`, `department`
- `developer`: `id`, `programming_language`

---

## 🧾 3. `TABLE_PER_CLASS` Strategy

### ✅ Description:
- Each concrete class has its **own table** with **all inherited fields**.

### 📦 Pros:
- No joins required.
- Fast inserts.

### ⚠️ Cons:
- Data duplication.
- No shared constraints.
- Complex polymorphic queries.

### 🧪 Example:

#### Base Class
```java
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public abstract class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    private String name;
}
```

#### Subclasses
```java
@Entity
public class Manager extends Employee {
    private String department;
}

@Entity
public class Developer extends Employee {
    private String programmingLanguage;
}
```

#### Resulting Tables:

- `manager`: `id`, `name`, `department`
- `developer`: `id`, `name`, `programming_language`

---

## 🧠 Best Practices

| Use Case | Recommended Strategy |
|----------|----------------------|
| Simple hierarchy, performance critical | `SINGLE_TABLE` |
| Normalized schema, moderate complexity | `JOINED` |
| Independent subclasses, no polymorphic queries | `TABLE_PER_CLASS` |

- Use `@DiscriminatorColumn` and `@DiscriminatorValue` with `SINGLE_TABLE`.
- Use `abstract` base classes when they are not meant to be instantiated directly.
- Always consider **query performance**, **data volume**, and **schema design** when choosing a strategy.

---
Performance considerations in **JPA inheritance mapping** are crucial because the chosen strategy directly affects how data is stored, retrieved, and queried. Here's a breakdown of how each strategy impacts performance, along with guidance on when to use each:

---

## 🚀 Performance Comparison of Inheritance Strategies

| Strategy            | Query Performance | Insert Performance | Storage Efficiency | Schema Complexity |
|---------------------|-------------------|--------------------|--------------------|-------------------|
| `SINGLE_TABLE`      | ✅ Fastest         | ✅ Fastest          | ❌ Inefficient (many nulls) | ✅ Simple |
| `JOINED`            | ❌ Slower (joins)  | ⚠️ Moderate         | ✅ Efficient        | ⚠️ Moderate |
| `TABLE_PER_CLASS`   | ✅ Fast queries    | ✅ Fast inserts     | ❌ Redundant data   | ❌ Complex |

---

### 🔹 `SINGLE_TABLE` Strategy

#### ✅ Best For:
- Small hierarchies
- High read performance
- Simple applications

#### ⚠️ Performance Notes:
- **Fast queries**: No joins required.
- **Insert/update**: Simple and fast.
- **Downside**: Many nullable columns can waste space and slow down scans on large tables.

---

### 🔸 `JOINED` Strategy

#### ✅ Best For:
- Medium to large hierarchies
- Normalized data
- Applications needing clean schema design

#### ⚠️ Performance Notes:
- **Slower queries**: Requires joins across multiple tables.
- **Efficient storage**: No null columns.
- **Good for reporting**: Clean separation of concerns.

---

### 🔻 `TABLE_PER_CLASS` Strategy

#### ✅ Best For:
- Independent subclasses
- No polymorphic queries
- Fast inserts

#### ⚠️ Performance Notes:
- **Fast inserts**: Each subclass writes to its own table.
- **Complex queries**: Hard to query across hierarchy.
- **Redundant schema**: Each table duplicates base class fields.

---

## 🧠 Strategy Selection Guidelines

| Criteria | Recommended Strategy |
|---------|----------------------|
| ✅ **Performance-critical reads** | `SINGLE_TABLE` |
| ✅ **Normalized schema, clean design** | `JOINED` |
| ✅ **Independent entities, fast inserts** | `TABLE_PER_CLASS` |
| ❌ **Polymorphic queries needed** | Avoid `TABLE_PER_CLASS` |
| ❌ **Large number of subclasses** | Avoid `SINGLE_TABLE` |

---

## 🛠️ Optimization Tips

- Use **indexes** on discriminator columns (`SINGLE_TABLE`) and foreign keys (`JOINED`).
- Use **lazy loading** for relationships to avoid unnecessary joins.
- Profile queries using **Hibernate statistics** or **JPA logging**.
- Consider **DTO projections** for read-heavy use cases.

---
Here’s a **benchmark comparison chart** of the three JPA inheritance strategies — **SINGLE_TABLE**, **JOINED**, and **TABLE_PER_CLASS** — across four key dimensions:

- **Query Performance**
- **Insert Performance**
- **Storage Efficiency**
- **Schema Complexity**

### 📊 Radar Chart Visualization

**Refer Radar-Chart-Visualization.png**
---

### 🔍 Insights from the Chart

| Strategy         | Strengths                          | Weaknesses                          |
|------------------|-------------------------------------|--------------------------------------|
| **SINGLE_TABLE** | Fast queries and inserts, simple schema | Poor storage efficiency (many nulls) |
| **JOINED**       | Efficient storage, normalized schema | Slower queries due to joins          |
| **TABLE_PER_CLASS** | Fast inserts, good query speed for subclasses | Redundant data, complex schema       |

---
