### 📘 Spring Data JPA: `CrudRepository` vs `JpaRepository`

Spring Data JPA is a powerful abstraction over JPA (Java Persistence API) that simplifies data access layers in Spring applications. It provides repository interfaces that reduce boilerplate code for common database operations.

---

## 🔹 1. `CrudRepository<T, ID>`

This is the **base interface** for generic CRUD operations on a repository for a specific type.

### ✅ Key Methods:
```java
<S extends T> S save(S entity);
Optional<T> findById(ID id);
Iterable<T> findAll();
void deleteById(ID id);
boolean existsById(ID id);
long count();
```

### 📌 Use Case:
Use `CrudRepository` when you only need **basic CRUD operations**.

---

## 🔸 2. `JpaRepository<T, ID>`

This extends `CrudRepository` and adds **JPA-specific** methods like batch operations and pagination.

### ✅ Additional Methods:
```java
List<T> findAll();
List<T> findAllById(Iterable<ID> ids);
void flush();
<S extends T> List<S> saveAllAndFlush(Iterable<S> entities);
void deleteAllInBatch();
```

### 📌 Use Case:
Use `JpaRepository` when you need:
- **Pagination and sorting**
- **Batch operations**
- **JPA-specific features**

---

## 🧩 Interface Hierarchy

```
Repository
 └── CrudRepository
      └── PagingAndSortingRepository
           └── JpaRepository
```

---

## 🧪 Example

### Entity:
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```

### Repository:
```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
}
```

---

## 🆚 Summary Table

| Feature                  | `CrudRepository` | `JpaRepository` |
|--------------------------|------------------|------------------|
| Basic CRUD               | ✅               | ✅               |
| Pagination & Sorting     | ❌               | ✅               |
| Batch Operations         | ❌               | ✅               |
| JPA-specific Features    | ❌               | ✅               |
| Custom Queries           | ✅               | ✅               |

---
Here are some **advanced features of JPA (Java Persistence API)** that go beyond basic CRUD operations and are essential for building complex, high-performance, and scalable applications:

---

## 🔍 1. **JPQL (Java Persistence Query Language)**
- Object-oriented query language similar to SQL but operates on entity objects.
- Supports joins, subqueries, grouping, and more.
```java
@Query("SELECT u FROM User u WHERE u.email = :email")
User findByEmail(@Param("email") String email);
```

---

## 🧩 2. **Criteria API**
- Type-safe, dynamic query construction using Java code.
- Useful for building complex queries programmatically.
```java
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<User> cq = cb.createQuery(User.class);
Root<User> root = cq.from(User.class);
cq.select(root).where(cb.equal(root.get("name"), "Alice"));
```

---

## 🗃️ 3. **Entity Relationships**
- `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`
- Cascade operations (`CascadeType.ALL`, etc.)
- Lazy vs Eager loading (`fetch = FetchType.LAZY`)

---

## 🧠 4. **Entity Lifecycle Callbacks**
- Hooks into entity lifecycle events:
  - `@PrePersist`, `@PostPersist`
  - `@PreUpdate`, `@PostUpdate`
  - `@PreRemove`, `@PostRemove`
  - `@PostLoad`

---

## 🧵 5. **Optimistic and Pessimistic Locking**
- Prevents data conflicts in concurrent environments.
- `@Version` for optimistic locking.
- `LockModeType.PESSIMISTIC_WRITE` for pessimistic locking.

---

## 📦 6. **Custom Repository Implementations**
- Extend Spring Data repositories with custom logic.
```java
public interface UserRepositoryCustom {
    List<User> findUsersWithCustomLogic();
}
```

---

## 📊 7. **Pagination and Sorting**
- Built-in support via `Pageable` and `Sort`.
```java
Page<User> users = userRepository.findAll(PageRequest.of(0, 10, Sort.by("name")));
```

---

## 🧰 8. **Auditing**
- Automatically track created/modified timestamps and users.
- Annotations: `@CreatedDate`, `@LastModifiedDate`, `@CreatedBy`, `@LastModifiedBy`

---

## 🧪 9. **Entity Graphs**
- Fine-grained control over fetching strategies.
```java
@EntityGraph(attributePaths = {"roles"})
List<User> findAllWithRoles();
```

---

## 🧬 10. **Inheritance Mapping**
- Map Java class hierarchies to database tables.
  - `@Inheritance(strategy = InheritanceType.SINGLE_TABLE)`
  - `JOINED`, `TABLE_PER_CLASS`

---

