Understanding the **JDK**, **JRE**, and **JVM** is fundamental to mastering Java. Here's a detailed breakdown of each:

---

## ☕ Java Virtual Machine (JVM)

### 🔹 What is it?
The **JVM** is the engine that runs Java applications. It interprets compiled Java bytecode and executes it on the host machine.

### 🔹 Key Responsibilities:
- **Loading** class files
- **Verifying** bytecode
- **Executing** bytecode
- **Managing memory** (via Garbage Collection)

### 🔹 Platform Independence:
Java code is compiled into **bytecode** (.class files), which the JVM can run on any platform (Windows, Linux, macOS), making Java **platform-independent**.

---

## 📦 Java Runtime Environment (JRE)

### 🔹 What is it?
The **JRE** is a package that contains the **JVM**, core libraries, and other components required to run Java applications.

### 🔹 Includes:
- JVM
- Java class libraries (e.g., java.lang, java.util)
- Supporting files (like configuration files)

### 🔹 Purpose:
If you're **only running** Java programs (not developing), the JRE is sufficient.

---

## 🛠️ Java Development Kit (JDK)

### 🔹 What is it?
The **JDK** is a full-featured software development kit for Java. It includes everything in the JRE **plus** tools for developing Java applications.

### 🔹 Includes:
- JRE (and thus the JVM)
- **Compiler** (`javac`)
- **Debugger** (`jdb`)
- **JavaDoc** tool
- Other development tools

### 🔹 Purpose:
If you're **writing and compiling** Java code, you need the JDK.

---

## 🔁 Relationship Between JDK, JRE, and JVM

```
JDK = JRE + Development Tools
JRE = JVM + Libraries
```

- **JVM** is the core engine.
- **JRE** is the runtime environment that includes the JVM.
- **JDK** is the full development kit that includes the JRE.

---
_**Note:** Refer Image/JDK-Block-Diagram.png for JDK, JRE_
