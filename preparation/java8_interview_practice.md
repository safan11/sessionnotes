
# Java 8 Interview Questions & Coding Practice 

---

## 📌 Introduction to Java 8

Java 8 introduced **functional programming concepts** to Java, making code:
- Cleaner
- More readable
- Less error-prone
- Easier to parallelize

---

## 🔹 1. Main Features Introduced in Java 8

### ✅ Key Features
- **Lambda Expressions** – Write functions inline
- **Stream API** – Functional-style data processing
- **Functional Interfaces** – Single abstract method interfaces
- **Optional** – Avoid `NullPointerException`
- **Default Methods** – Method implementation inside interfaces
- **Date & Time API** – Immutable, thread-safe date handling
- **Method References** – Shorthand for lambdas
- **Parallel Streams** – Multi-threaded data processing

---

## 🔹 2. What are Functional Interfaces?

### 📌 Definition
A **functional interface** has:
- Exactly **ONE abstract method**
- Can have multiple default/static methods

### 📌 Why?
- Enables **lambda expressions**
- Enables **method references**

### 📌 Examples
```java
Runnable        -> void run()
Predicate<T>    -> boolean test(T t)
Function<T, R>  -> R apply(T t)
Consumer<T>     -> void accept(T t)
````

---

## 🔹 3. Stream API Explained

### 📌 What is a Stream?

A **Stream** is a sequence of elements supporting:

* Functional operations
* Lazy evaluation
* No data storage (works on collections)

---

### 🔹 Stream Operations

#### 🔸 Intermediate Operations (Return Stream)

* `filter()`
* `map()`
* `sorted()`
* `distinct()`

#### 🔸 Terminal Operations (End Stream)

* `collect()`
* `forEach()`
* `reduce()`
* `findFirst()`
* `count()`

---

## 🔹 4. map() vs flatMap()

| map()                     | flatMap()              |
| ------------------------- | ---------------------- |
| One-to-one mapping        | One-to-many flattening |
| Returns Stream<Stream<T>> | Returns Stream<T>      |

### Example

```java
List<List<Integer>> list = Arrays.asList(
  Arrays.asList(1,2),
  Arrays.asList(3,4)
);

list.stream()
    .flatMap(List::stream)
    .forEach(System.out::println);
```

---

## 🔹 5. Optional Class

### 📌 Purpose

* Avoid `NullPointerException`

### 📌 Methods

```java
Optional.of(value)
Optional.empty()
Optional.ofNullable(value)
ifPresent()
orElse()
orElseThrow()
```

---

## 🔹 6. Default Methods in Interfaces

### 📌 Why?

* Add new methods without breaking old implementations

```java
interface MyInterface {
    default void show() {
        System.out.println("Default Method");
    }
}
```

---

## 🔹 7. Collectors Utility

### 📌 Purpose

Used with `collect()` to convert streams.

### Common Collectors

* `toList()`
* `toSet()`
* `joining()`
* `groupingBy()`
* `partitioningBy()`
* `counting()`

---

## 🔹 8. Java 8 Date & Time API

### Problems with Old API

* Mutable
* Not thread-safe

### Java 8 Solution

```java
LocalDate
LocalTime
LocalDateTime
ZonedDateTime
DateTimeFormatter
```

---

## 🔹 9. Method References

### Types

```java
ClassName::staticMethod
object::instanceMethod
ClassName::new
```

---

## 🔹 10. parallelStream()

### 📌 Purpose

* Multi-threaded processing
* Improves performance on large datasets

```java
list.parallelStream().forEach(System.out::println);
```

---

# 🔷 CODING QUESTIONS WITH FULL CODE

---

## 1️⃣ Print List Using Lambda

```java
List<String> names = Arrays.asList("Alice","Bob","Charlie");
names.forEach(System.out::println);
```

---

## 2️⃣ Filter Even Numbers

```java
List<Integer> numbers = Arrays.asList(1,2,3,4,5,6);

List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

System.out.println(evens);
```

---

## 3️⃣ Find Maximum Value

```java
int max = numbers.stream()
    .max(Integer::compare)
    .orElse(0);
```

---

## 4️⃣ Convert to Uppercase

```java
List<String> upper = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

---

## 5️⃣ Group by Length

```java
Map<Integer, List<String>> map =
names.stream().collect(Collectors.groupingBy(String::length));
```

---

## 6️⃣ Sum Using reduce()

```java
int sum = numbers.stream().reduce(0, Integer::sum);
```

---

## 7️⃣ Count Word Occurrences

```java
Map<String, Long> count =
words.stream()
.collect(Collectors.groupingBy(w -> w, Collectors.counting()));
```

---

## 8️⃣ Join Strings

```java
String result = words.stream()
.collect(Collectors.joining(" "));
```

---

## 9️⃣ Sort Employees by Salary

```java
class Employee {
    String name;
    int salary;
    Employee(String n, int s) {
        name = n; salary = s;
    }
}

employees.stream()
.sorted(Comparator.comparingInt(e -> e.salary))
.forEach(e -> System.out.println(e.name));
```

---

## 🔟 First Non-Repeated Character

```java
String input = "swiss";

Character result = input.chars()
.mapToObj(c -> (char)c)
.filter(ch -> input.indexOf(ch) == input.lastIndexOf(ch))
.findFirst()
.orElse(null);
```

---

## 🔹 findFirst() vs findAny()

| findFirst()     | findAny()        |
| --------------- | ---------------- |
| Ordered streams | Parallel streams |
| Deterministic   | Faster           |

---

## 🔹 Best Practices for Streams

* Avoid streams for very small collections
* Use `parallelStream()` only for large datasets
* Prefer method references
* Avoid modifying external variables

---

## 📌 Interview Tip Section

✔ Always explain **WHY streams are used**
✔ Mention **lazy evaluation**
✔ Explain **immutability**
✔ Know **Collectors** well
✔ Understand **map vs flatMap**

---

## ✅ Final Advice

> If you can confidently explain **Streams + Lambda + Collectors**,
> **80% of Java 8 interviews are covered.**

---



