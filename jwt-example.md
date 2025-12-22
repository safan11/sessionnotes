
# 🧠 BIG PICTURE (UPDATED – SAME MEANING)

> **This application shows how:**
>
> 1. User logs in
> 2. System creates a JWT token
> 3. User sends token with every request
> 4. Spring Security checks the token
> 5. Role decides access

---

# 📦 UPDATED PACKAGE STRUCTURE

```
com.tcs
│
├── JwtAppApplication.java
│
├── service
│   └── CustomUserDetailsService.java
│
├── util
│   └── JwtUtil.java
│
├── config
│   ├── JwtAuthenticationFilter.java
│   └── SecurityConfig.java
│
├── model
│   ├── AuthRequest.java
│   └── AuthResponse.java
│
├── controller
│   ├── AuthController.java
│   └── DemoController.java
```

---

# 1️⃣ MAIN APPLICATION CLASS

```java
package com.tcs;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class JwtAppApplication {

    public static void main(String[] args) {
        SpringApplication.run(JwtAppApplication.class, args);
    }
}
```

---

# 2️⃣ USER DETAILS SERVICE

```java
package com.tcs.service;

import org.springframework.security.core.userdetails.*;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String username)
            throws UsernameNotFoundException {

        if ("user1".equals(username)) {
            return new User(
                    "user1",
                    "{noop}user123",
                    List.of(new SimpleGrantedAuthority("ROLE_USER"))
            );
        }

        if ("admin".equals(username)) {
            return new User(
                    "admin",
                    "{noop}admin123",
                    List.of(new SimpleGrantedAuthority("ROLE_ADMIN"))
            );
        }

        throw new UsernameNotFoundException("User not found");
    }
}
```

---

# 3️⃣ JWT UTILITY CLASS

```java
package com.tcs.util;

import io.jsonwebtoken.*;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class JwtUtil {

    private final String SECRET_KEY = "tcs-secret-key";

    public String generateToken(String username) {

        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(
                        new Date(System.currentTimeMillis() + 60 * 60 * 1000)
                )
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
                .compact();
    }

    public String extractUsername(String token) {

        return Jwts.parser()
                .setSigningKey(SECRET_KEY)
                .parseClaimsJws(token)
                .getBody()
                .getSubject();
    }
}
```

---

# 4️⃣ AUTH REQUEST MODEL

```java
package com.tcs.model;

public class AuthRequest {

    private String username;
    private String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

---

# 5️⃣ AUTH RESPONSE MODEL

```java
package com.tcs.model;

public class AuthResponse {

    private String token;

    public AuthResponse(String token) {
        this.token = token;
    }

    public String getToken() {
        return token;
    }
}
```

---

# 6️⃣ JWT FILTER (UPDATED – CORRECT WAY)

```java
package com.tcs.config;

import com.tcs.util.JwtUtil;
import com.tcs.service.CustomUserDetailsService;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.web.filter.OncePerRequestFilter;
import org.springframework.stereotype.Component;

import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtUtil jwtUtil;
    private final CustomUserDetailsService userService;

    public JwtAuthenticationFilter(
            JwtUtil jwtUtil,
            CustomUserDetailsService userService) {

        this.jwtUtil = jwtUtil;
        this.userService = userService;
    }

    @Override
    protected void doFilterInternal(
            HttpServletRequest request,
            HttpServletResponse response,
            FilterChain filterChain)
            throws ServletException, IOException {

        String authHeader = request.getHeader("Authorization");

        if (authHeader != null && authHeader.startsWith("Bearer ")) {

            String token = authHeader.substring(7);
            String username = jwtUtil.extractUsername(token);

            var userDetails =
                    userService.loadUserByUsername(username);

            var authToken =
                    new UsernamePasswordAuthenticationToken(
                            userDetails,
                            null,
                            userDetails.getAuthorities()
                    );

            authToken.setDetails(
                    new WebAuthenticationDetailsSource()
                            .buildDetails(request)
            );

            SecurityContextHolder.getContext()
                    .setAuthentication(authToken);
        }

        filterChain.doFilter(request, response);
    }
}
```

---

# 7️⃣ SECURITY CONFIGURATION

```java
package com.tcs.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(
            HttpSecurity http,
            JwtAuthenticationFilter jwtFilter
    ) throws Exception {

        http.csrf(csrf -> csrf.disable());

        http.authorizeHttpRequests(auth -> auth
                .requestMatchers("/login").permitAll()
                .requestMatchers("/user").hasRole("USER")
                .requestMatchers("/admin").hasRole("ADMIN")
                .anyRequest().authenticated()
        );

        http.addFilterBefore(
                jwtFilter,
                UsernamePasswordAuthenticationFilter.class
        );

        return http.build();
    }
}
```

---

# 8️⃣ AUTH CONTROLLER

```java
package com.tcs.controller;

import com.tcs.model.*;
import com.tcs.util.JwtUtil;
import org.springframework.web.bind.annotation.*;

@RestController
public class AuthController {

    private final JwtUtil jwtUtil;

    public AuthController(JwtUtil jwtUtil) {
        this.jwtUtil = jwtUtil;
    }

    @PostMapping("/login")
    public AuthResponse login(@RequestBody AuthRequest request) {

        String token =
                jwtUtil.generateToken(request.getUsername());

        return new AuthResponse(token);
    }
}
```

---

# 9️⃣ PROTECTED CONTROLLER

```java
package com.tcs.controller;

import org.springframework.web.bind.annotation.*;

@RestController
public class DemoController {

    @GetMapping("/user")
    public String userApi() {
        return "Hello USER – Access Granted";
    }

    @GetMapping("/admin")
    public String adminApi() {
        return "Hello ADMIN – Access Granted";
    }
}
```

---

# 📦 pom.xml (IMPORTANT)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.tcs</groupId>
    <artifactId>jwt-app</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
    </parent>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>

        <!-- Spring Web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Security -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <!-- JWT -->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>

        <!-- Test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

---

Below is a **CLEAR, STEP-BY-STEP EXECUTION FLOW** showing:

* 🔁 **Which file is called**
* 📍 **Where control goes**
* 🧠 **Why that file exists**
* 🔐 **How JWT + Spring Security interact**

---

# 🔄 COMPLETE APPLICATION FLOW (END-TO-END)

---

## 🟢 STEP 0 — APPLICATION STARTS

### ▶ File Called

```
com.tcs.JwtAppApplication
```

```java
@SpringBootApplication
public class JwtAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(JwtAppApplication.class, args);
    }
}
```

### 🔹 What Happens Internally

* Spring Boot starts server (Tomcat)
* Spring scans packages under `com.tcs`
* Creates beans for:

  * `JwtUtil`
  * `JwtAuthenticationFilter`
  * `CustomUserDetailsService`
  * `SecurityConfig`
  * Controllers

✅ Application is now **READY to receive requests**

---

## 🟡 STEP 1 — USER SENDS LOGIN REQUEST

### 📩 Request

```
POST /login
{
  "username": "user1",
  "password": "user123"
}
```

---

## 🟡 STEP 2 — SECURITY FILTER CHECK (FIRST GATE)

### ▶ File Called

```
SecurityConfig
```

```java
.requestMatchers("/login").permitAll()
```

### 🔹 Decision

* `/login` is **PUBLIC**
* ❌ JWT filter is **NOT required**
* Request moves forward

---

## 🟡 STEP 3 — AUTH CONTROLLER HANDLES LOGIN

### ▶ File Called

```
AuthController
```

```java
@PostMapping("/login")
public AuthResponse login(@RequestBody AuthRequest request)
```

### 🔹 Flow Inside Controller

1️⃣ Request body mapped to:

```
AuthRequest
```

2️⃣ Controller calls:

```
JwtUtil.generateToken(username)
```

---

## 🟡 STEP 4 — JWT TOKEN IS CREATED

### ▶ File Called

```
JwtUtil
```

```java
public String generateToken(String username)
```

### 🔹 Token Contains

* username
* issued time
* expiry time
* secret signature

### 📤 Response Sent Back

```
AuthResponse
{
  "token": "eyJhbGciOiJIUzI1NiJ9..."
}
```

✅ **Login Flow Ends Here**

---

# 🔐 FROM NOW ON — PROTECTED REQUEST FLOW

---

## 🔵 STEP 5 — USER CALLS PROTECTED API

### 📩 Request

```
GET /user
Authorization: Bearer <JWT_TOKEN>
```

---

## 🔵 STEP 6 — JWT FILTER EXECUTES (MOST IMPORTANT)

### ▶ File Called

```
JwtAuthenticationFilter
```

This filter runs **BEFORE controller**.

---

### 🔍 Inside JwtAuthenticationFilter

```java
String authHeader = request.getHeader("Authorization");
```

#### ✔ Token Exists?

* Yes → Continue
* No → Request rejected later

---

### 🔍 Token Processing

```java
String token = authHeader.substring(7);
String username = jwtUtil.extractUsername(token);
```

### ▶ File Called

```
JwtUtil
```

✔ Username extracted from token

---

## 🔵 STEP 7 — LOAD USER DETAILS

### ▶ File Called

```
CustomUserDetailsService
```

```java
loadUserByUsername(username)
```

### 🔹 What It Returns

```
UserDetails {
  username
  password
  roles
}
```

✔ Roles are attached (`ROLE_USER` / `ROLE_ADMIN`)

---

## 🔵 STEP 8 — AUTHENTICATION STORED IN CONTEXT

### ▶ File Used

```
SecurityContextHolder
```

```java
SecurityContextHolder.getContext()
    .setAuthentication(authentication);
```

### 🔹 Meaning

> “This request is AUTHENTICATED”

Now Spring **trusts this user**

---

## 🔵 STEP 9 — SECURITY RULES CHECKED

### ▶ File Called

```
SecurityConfig
```

```java
.requestMatchers("/user").hasRole("USER")
```

### 🔹 Decision

* Token valid? ✅
* User has ROLE_USER? ✅
* Access granted 🎉

---

## 🔵 STEP 10 — CONTROLLER METHOD EXECUTES

### ▶ File Called

```
DemoController
```

```java
@GetMapping("/user")
public String userApi()
```

### 📤 Response

```
Hello USER – Access Granted
```

---

# 🟥 ADMIN FLOW (SAME PIPELINE)

```
GET /admin
Authorization: Bearer <JWT>
```

### Checks

| Check       | File                     |
| ----------- | ------------------------ |
| JWT present | JwtAuthenticationFilter  |
| Token valid | JwtUtil                  |
| User loaded | CustomUserDetailsService |
| Role check  | SecurityConfig           |
| Controller  | DemoController           |

❌ USER → denied
✅ ADMIN → allowed

---

# 🧠 FILE RESPONSIBILITY MAP (VERY IMPORTANT)

| File                       | Responsibility     |
| -------------------------- | ------------------ |
| `JwtAppApplication`        | Starts application |
| `SecurityConfig`           | Security rules     |
| `JwtAuthenticationFilter`  | Token validation   |
| `JwtUtil`                  | Create & read JWT  |
| `CustomUserDetailsService` | Load users & roles |
| `AuthController`           | Login              |
| `AuthRequest`              | Login input        |
| `AuthResponse`             | Token output       |
| `DemoController`           | Protected APIs     |

---

# 🧩 COMPLETE FLOW IN ONE LINE (INTERVIEW GOLD)

> **Request → Security Filter → JWT Validation → User Loading → Role Check → Controller**

---

# 🏁 FINAL REAL-WORLD STATEMENT

> **JWT replaces session.
> Spring Security validates identity on every request.
> Roles decide access.**

---


