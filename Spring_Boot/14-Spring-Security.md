Spring Security is a powerful and customizable **authentication and access-control framework** for Java applications, especially those built with **Spring Boot**. It provides comprehensive security services for Java EE-based enterprise software applications.

---

## 🔐 **Core Concepts of Spring Security**

### 1. **Authentication**
Verifying the identity of a user (e.g., username/password).

### 2. **Authorization**
Determining whether a user has permission to access a resource.

### 3. **Security Context**
Holds the authentication and authorization information of the current user.

### 4. **Filters**
Spring Security uses a chain of filters to intercept and process security-related requests.

### 5. **UserDetailsService**
Interface used to retrieve user-related data for authentication.

---

## 🧱 **Spring Security Architecture Overview**

```
Client Request
     ↓
Security Filter Chain
     ↓
Authentication Manager
     ↓
UserDetailsService → UserDetails
     ↓
Access Decision Manager
     ↓
Controller / Resource
```

---

## ⚙️ **Basic Setup in Spring Boot**

### **Step 1: Add Dependency**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### **Step 2: Default Behavior**
Once added, Spring Security:
- Secures all endpoints by default
- Requires basic authentication (username/password)

Default credentials:
- Username: `user`
- Password: auto-generated in console

---

## 🔐 **Custom Security Configuration**

### **Example: Securing Endpoints**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
                .anyRequest().permitAll()
            )
            .formLogin(Customizer.withDefaults())
            .httpBasic(Customizer.withDefaults());
        return http.build();
    }
}
```

---

## 👤 **Custom UserDetailsService**

```java
@Service
public class MyUserDetailsService implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // Example user
        return User.withUsername("modinshalli")
                   .password("{noop}password") // {noop} means no encoding
                   .roles("USER")
                   .build();
    }
}
```

---

## 🔐 **Password Encoding**

Spring Security requires passwords to be encoded. Use `PasswordEncoder`:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

---

## 🧪 **Testing with Postman or Curl**

```bash
curl -u modinshalli:password http://localhost:8080/user/profile
```

---

## 🛡️ **Advanced Features**

| Feature | Description |
|--------|-------------|
| JWT Authentication | Token-based stateless authentication |
| OAuth2 | Integration with Google, Facebook, GitHub, etc. |
| CSRF Protection | Prevents cross-site request forgery |
| Method Security | Use `@PreAuthorize`, `@Secured` on methods |
| Session Management | Control session creation, timeout, etc. |

---

## 📘 **Summary**

Spring Security provides:
- **Authentication & Authorization**
- **Secure defaults**
- **Customizable filters and user services**
- **Integration with modern protocols (JWT, OAuth2)**

---
