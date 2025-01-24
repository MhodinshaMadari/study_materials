**1. What is a String in Java?**

* **Definition:** In Java, a String is an object that represents a sequence of characters. 
* **Immutability:** Strings are immutable, meaning their values cannot be changed after creation. Any operation that appears to modify a string actually creates a new string object.

**2. How do you create a String in Java?**

* **Using String Literals:**
   ```java
   String str1 = "Hello"; 
   ```
* **Using the `new` keyword:**
   ```java
   String str2 = new String("Hello"); 
   ```

**3. What is the String Pool in Java?**

* The String pool is a special memory area in the Java heap where String objects created using literals are stored. 
* If a String literal already exists in the pool, a reference to the existing object is returned instead of creating a new one.

**4. What is the difference between `==` and `equals()` for String comparison?**

* **`==`:** Checks for object equality (whether the two String objects refer to the same memory location).
* **`equals()`:** Checks for content equality (whether the two strings have the same sequence of characters).

**5. When should you use `StringBuilder` or `StringBuffer` instead of `String`?**

* When you need to perform frequent modifications to a string (e.g., concatenations, insertions, deletions), use `StringBuilder` or `StringBuffer`.
* `StringBuilder` is faster but not thread-safe. 
* `StringBuffer` is thread-safe but slower.

**6. How do you check if a string is empty or null?**

* **Check for null:**
   ```java
   if (str == null) { 
       // Handle null string
   }
   ```
* **Check for empty string:**
   ```java
   if (str.isEmpty()) { 
       // Handle empty string
   }
   ```
* **Check for both:**
   ```java
   if (str == null || str.isEmpty()) { 
       // Handle null or empty string
   }
   ```

**7. How do you convert a String to an integer in Java?**

* Use the `Integer.parseInt()` method:

   ```java
   String str = "123";
   int num = Integer.parseInt(str); 
   ```

**8. How do you reverse a String in Java?**

* **Using StringBuilder:**

   ```java
   String str = "hello";
   String reversedStr = new StringBuilder(str).reverse().toString();
   ```

* **Using a loop:**

   ```java
   String str = "hello";
   String reversedStr = "";
   for (int i = str.length() - 1; i >= 0; i--) {
       reversedStr += str.charAt(i);
   }
   ```

**9. What are some common String methods in Java?**

* `length()`
* `charAt()`
* `substring()`
* `toLowerCase()`
* `toUpperCase()`
* `trim()`
* `concat()`
* `indexOf()`
* `lastIndexOf()`
* `contains()`
* `split()`
* `replace()`

**10. Why is String immutable in Java?**

* **Security:** Protects sensitive data from unauthorized modification.
* **Caching:** Allows for efficient string pooling and memory optimization.
* **Multithreading:** Makes strings thread-safe without the need for external synchronization.
* **Hashing:** Ensures that the hash code of a string remains constant, which is crucial for hash-based data structures.

These are some of the most frequently asked interview questions about Strings in Java. Remember to practice coding examples and understand the underlying concepts thoroughly.
