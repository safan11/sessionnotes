# 📘 **Complete Guide to List in Java — By *Safan Codes***

### 👤 **Author:** Safan Codes
---

# 📚 **TABLE OF CONTENTS**

1. 📜 Introduction to List
2. ⭐ Features of List
3. 🧩 Implementations
4. 🛠 CRUD Operations
5. 🔁 Iteration
6. 📦 Bulk Operations
7. 🧮 Collections Utility Methods
8. 📊 Sorting & Reverse
9. ⚔ ArrayList vs LinkedList
10. ⚙ Internal Working (Dynamic Array Growth)
11. 📑 Programs with Generics
12. 📘 Book Management Mini Project

---

# 📜 **1. Introduction to List**

`List` is an interface in Java that:

* 📌 Maintains **order**
* 📌 Allows **duplicates**
* 📌 Supports **index-based access**
* 📌 Belongs to `java.util` package

Example:

```java
List<String> names = new ArrayList<>();
```

---

# ⭐ **2. Features of List**

* ➕ Insert elements
* ➖ Remove elements
* 📝 Update elements
* 🔍 Search elements
* 📥 Supports `null`
* 🪝 Supports iterators
* 🧹 Works with `Collections` utility

---

# 🧩 **3. List Implementations**

### 🔹 **ArrayList**

### 🔹 **LinkedList**

### 🔹 **Vector**

### 🔹 **Stack**

---

# 🔹 **3.1 ArrayList**

### 🧠 Internal Working (Dynamic Array)

ArrayList uses a **dynamic array**.

```
Initial Capacity = 10  
When full → newCapacity = old * 1.5
```

### 📈 Capacity Expansion Animation

```
Before:
[10][20][30][40][50][60][70][80][90][100]  cap=10

Add new element → Resize triggered

After:
[10][20][30][40][50][60][70][80][90][100][101]...... cap=15
```

### 👍 Best For

* Fast read (O(1))
* Random access

### 👎 Not Good For

* Middle insert/delete (O(n))

---

# 🔹 **3.2 LinkedList**

### 🪢 Node Structure

```
NULL ← [10] ↔ [20] ↔ [30] → NULL
```

### 👍 Best For

* Frequent addition/removal

### 👎 Not Good For

* Random access (O(n))

---

# 🔹 **3.3 Vector**

* 🛡 Thread-safe
* 🐌 Slower
* 📜 Legacy class

---

# 🔹 **3.4 Stack**

* 📚 LIFO
* push(), pop(), peek()

---

# 🛠 **4. CRUD Operations**

```java
List<String> fruits = new ArrayList<>();

// Create
fruits.add("Apple");

// Read
System.out.println(fruits.get(0));

// Update
fruits.set(0, "Mango");

// Delete
fruits.remove("Mango");
```

---

# 🔁 **5. Iteration Methods**

### 1️⃣ **for loop**

```java
for(int i=0;i<list.size();i++){
    System.out.println(list.get(i));
}
```

### 2️⃣ **Enhanced for**

```java
for(String s : list) {}
```

### 3️⃣ **Iterator**

```java
Iterator<String> it = list.iterator();
```

### 4️⃣ **ListIterator**

```java
ListIterator<String> it = list.listIterator();
```

### 5️⃣ **forEach + Lambda**

```java
list.forEach(i -> System.out.println(i));
```

---

# 📦 **6. Bulk Operations**

### ➕ addAll()

```java
list1.addAll(list2);
```

### 🧹 removeAll()

```java
list1.removeAll(list2);
```

### 🔒 retainAll()

```java
list1.retainAll(list2);
```

---

# 🧮 **7. Collections Utility Methods**

### 📊 Sorting Ascending

```java
Collections.sort(list);
```

### 🔄 Reverse Order

```java
Collections.sort(list, Collections.reverseOrder());
```

### 🔁 Reverse Only

```java
Collections.reverse(list);
```

---

# ⚔ **8. ArrayList vs LinkedList**

| Feature       | ArrayList     | LinkedList          |
| ------------- | ------------- | ------------------- |
| Storage       | Dynamic Array | Doubly Linked Nodes |
| Access        | O(1)          | O(n)                |
| Insert Middle | Slow          | Fast                |
| Memory        | Low           | High                |
| Best For      | Access        | Insert/Delete       |

---

# ⚙ **9. Internal Working of ArrayList (Dynamic Growth)**

### 🔧 Step 1: Initial Capacity = 10

### 🔧 Step 2: When full → newCapacity = oldCapacity * 1.5

### 🔧 Step 3: Elements copied into new array

```
Old Array (size=10)
[0][1][2][3][4][5][6][7][8][9]

Resize → Capacity = 15

[0][1][2][3][4][5][6][7][8][9][ ][ ][ ][ ][ ]
```

---

# 📑 **10. Programs with Generics**

### ✔ Simple Generic List Example

```java
List<Integer> nums = new ArrayList<>();
nums.add(10);
nums.add(20);

for(Integer n : nums){
    System.out.println(n);
}
```

---

# 📘 **11. Mini Project: Book Management Application**

Uses:
📗 **Model**
🛠 **Service**
🖥 **Client**

---

## 📗 **Book.java (Model)**

```java
public class Book {
    private int id;
    private String title;
    private String author;

    public Book(int id, String title, String author){
        this.id = id;
        this.title = title;
        this.author = author;
    }

    public int getId(){ return id; }
    public String getTitle(){ return title; }
    public String getAuthor(){ return author; }

    @Override
    public String toString(){
        return id + " - " + title + " - " + author;
    }
}
```

---

## 🛠 **BookService.java**

```java
import java.util.ArrayList;
import java.util.List;

public class BookService {

    private List<Book> books = new ArrayList<>();

    public void addBook(Book b){
        books.add(b);
    }

    public List<Book> getAllBooks(){
        return books;
    }

    public boolean removeBook(int id){
        return books.removeIf(b -> b.getId() == id);
    }

    public Book findBook(int id){
        for(Book b : books){
            if(b.getId() == id) return b;
        }
        return null;
    }
}
```

---

## 🖥 **Client.java**

```java
public class Client {
    public static void main(String[] args) {

        BookService service = new BookService();

        service.addBook(new Book(1, "Java Basics", "Safan"));
        service.addBook(new Book(2, "Spring Boot", "Safan Ahmed"));

        System.out.println("📚 All Books:");
        service.getAllBooks().forEach(System.out::println);

        System.out.println("\n🔍 Find Book (1):");
        System.out.println(service.findBook(1));

        System.out.println("\n❌ Removing Book 2...");
        service.removeBook(2);

        System.out.println("\n📚 Updated Book List:");
        service.getAllBooks().forEach(System.out::println);
    }
}
```


# 🧪 **Sample Programs on List (ArrayList, LinkedList, Vector, Stack)**

---

# 🍏 **1. ArrayList — Student Marks Processing**

### **📘 Description**

Real-life usage of ArrayList to store student marks, calculate average, and print results.

---

## **💡 Sample Code — StudentMarksList.java**

```java
import java.util.ArrayList;

public class StudentMarksList {
    public static void main(String[] args) {

        ArrayList<Integer> marks = new ArrayList<>();

        marks.add(87);
        marks.add(92);
        marks.add(76);
        marks.add(95);

        int total = 0;
        for (int m : marks) {
            total += m;
        }

        double avg = total / (double) marks.size();

        System.out.println("Marks: " + marks);
        System.out.println("Average Marks: " + avg);
    }
}
```

## **📤 Output**

```
Marks: [87, 92, 76, 95]
Average Marks: 87.5
```

---

# 🍏 **2. ArrayList — Product List & Sorting**

### **📘 Description**

Store product prices and sort them in ascending order using Collections.sort().

---

## **💡 Sample Code — ProductPriceList.java**

```java
import java.util.ArrayList;
import java.util.Collections;

public class ProductPriceList {
    public static void main(String[] args) {

        ArrayList<Double> prices = new ArrayList<>();

        prices.add(1999.0);
        prices.add(499.0);
        prices.add(1299.0);
        prices.add(799.0);

        Collections.sort(prices);

        System.out.println("Sorted Prices: " + prices);
    }
}
```

## **📤 Output**

```
Sorted Prices: [499.0, 799.0, 1299.0, 1999.0]
```

---

# 🔵 **3. LinkedList — Task Scheduler**

### **📘 Description**

LinkedList is great for adding/removing tasks from beginning/end.

---

## **💡 Sample Code — TaskScheduler.java**

```java
import java.util.LinkedList;

public class TaskScheduler {
    public static void main(String[] args) {

        LinkedList<String> tasks = new LinkedList<>();

        tasks.addFirst("Initialize Server");
        tasks.add("Load Configurations");
        tasks.addLast("Start Services");

        System.out.println("Tasks Queue: " + tasks);

        tasks.removeFirst();  // Initial task done

        System.out.println("Updated Tasks: " + tasks);
    }
}
```

## **📤 Output**

```
Tasks Queue: [Initialize Server, Load Configurations, Start Services]
Updated Tasks: [Load Configurations, Start Services]
```

---

# 🔵 **4. LinkedList — Playlist Manager**

### **📘 Description**

Use LinkedList to create a music playlist where songs can be added or removed easily.

---

## **💡 Sample Code — PlaylistManager.java**

```java
import java.util.LinkedList;

public class PlaylistManager {
    public static void main(String[] args) {

        LinkedList<String> playlist = new LinkedList<>();

        playlist.add("Shape of You");
        playlist.add("Believer");
        playlist.add("Perfect");

        playlist.addFirst("Intro Track");
        playlist.addLast("Outro Track");

        System.out.println("Playlist: " + playlist);

        playlist.removeLast(); // Remove outro

        System.out.println("After Removing Outro: " + playlist);
    }
}
```

## **📤 Output**

```
Playlist: [Intro Track, Shape of You, Believer, Perfect, Outro Track]
After Removing Outro: [Intro Track, Shape of You, Believer, Perfect]
```

---

# 🟣 **VECTOR PROGRAMS**

📝 *Note: Vector = Same as ArrayList, but fully synchronized (thread-safe). Uses dynamic array internally.*

---

# 🟣 **5. Vector — Notification System**

### **📘 Description**

Store notifications and process them in order.

---

## **💡 Sample Code — NotificationVector.java**

```java
import java.util.Vector;

public class NotificationVector {
    public static void main(String[] args) {

        Vector<String> notifications = new Vector<>();

        notifications.add("New Message");
        notifications.add("Friend Request");
        notifications.add("Security Alert");

        System.out.println("Notifications: " + notifications);

        notifications.remove("Friend Request");

        System.out.println("After Processing: " + notifications);
        System.out.println("Vector Capacity: " + notifications.capacity());
    }
}
```

## **📤 Output**

```
Notifications: [New Message, Friend Request, Security Alert]
After Processing: [New Message, Security Alert]
Vector Capacity: 10
```

---

# 🟣 **6. Vector — EmployeeList with Enumeration**

### **📘 Description**

Enumerate vector elements (legacy but useful in some systems).

---

## **💡 Sample Code — EmployeeVector.java**

```java
import java.util.*;

public class EmployeeVector {
    public static void main(String[] args) {

        Vector<String> employees = new Vector<>();
        employees.add("Safan");
        employees.add("Ahmed");
        employees.add("John");

        Enumeration<String> e = employees.elements();

        System.out.println("Employee Names:");
        while (e.hasMoreElements()) {
            System.out.println(e.nextElement());
        }
    }
}
```

## **📤 Output**

```
Employee Names:
Safan
Ahmed
John
```

---

# 🟣 **7. Vector — Searching Products**

### **📘 Description**

Use contains() to check if product exists in vector.

---

## **💡 Sample Code — ProductSearchVector.java**

```java
import java.util.Vector;

public class ProductSearchVector {
    public static void main(String[] args) {

        Vector<String> products = new Vector<>();

        products.add("Laptop");
        products.add("Mouse");
        products.add("Keyboard");

        System.out.println("Contains Laptop? " + products.contains("Laptop"));
        System.out.println("Contains Charger? " + products.contains("Charger"));
    }
}
```

## **📤 Output**

```
Contains Laptop? true
Contains Charger? false
```

---

# 🔴 **STACK PROGRAMS**

Stack = LIFO
Real-life usage includes **Undo system, Browser history, Expression evaluation, Page navigation**, etc.

---

# 🔴 **8. Stack — Browser History**

### **📘 Description**

Stack stores visited pages. Last visited = first removed.

---

## **💡 Sample Code — BrowserHistoryStack.java**

```java
import java.util.Stack;

public class BrowserHistoryStack {
    public static void main(String[] args) {

        Stack<String> history = new Stack<>();

        history.push("Home Page");
        history.push("Products Page");
        history.push("Product Details");
        history.push("Checkout");

        System.out.println("Current Page: " + history.peek());

        System.out.println("Going Back...");
        history.pop();

        System.out.println("Now At: " + history.peek());
    }
}
```

## **📤 Output**

```
Current Page: Checkout
Going Back...
Now At: Product Details
```

---

# 🔴 **9. Stack — Undo Operation (Real-life Editing App)**

### **📘 Description**

Every action pushed into stack. Undo removes last action.

---

## **💡 Sample Code — UndoSystemStack.java**

```java
import java.util.Stack;

public class UndoSystemStack {
    public static void main(String[] args) {

        Stack<String> actions = new Stack<>();

        actions.push("Typed Hello");
        actions.push("Bold Applied");
        actions.push("Color Changed");

        System.out.println("Actions: " + actions);

        System.out.println("Undo Action: " + actions.pop());

        System.out.println("Remaining Actions: " + actions);
    }
}
```

## **📤 Output**

```
Actions: [Typed Hello, Bold Applied, Color Changed]
Undo Action: Color Changed
Remaining Actions: [Typed Hello, Bold Applied]
```

---

# 🔴 **10. Stack — Customer Service Calls (Last-One Attended First)**

### **📘 Description**

Customer calls stored, last entered call handled first.

---

## **💡 Sample Code — CustomerCallStack.java**

```java
import java.util.Stack;

public class CustomerCallStack {
    public static void main(String[] args) {

        Stack<String> callStack = new Stack<>();

        callStack.push("Call 101");
        callStack.push("Call 102");
        callStack.push("Call 103");

        System.out.println("Next Call To Attend: " + callStack.peek());

        System.out.println("Attending: " + callStack.pop());

        System.out.println("Calls Left: " + callStack);
    }
}
```

## **📤 Output**

```
Next Call To Attend: Call 103
Attending: Call 103
Calls Left: [Call 101, Call 102]
```

---

#  **Practice Questions & Answers on List and Its Classes**

---

### **1. What is a List in Java and how is it different from a Set?**

A **List** is an ordered collection that allows duplicate elements and supports index-based access.
A **Set** does not maintain order and does not allow duplicates.

---

### **2. Can we store duplicate elements inside a List? Why?**

Yes.
List allows duplicates because it stores elements based on **index**, not uniqueness.

---

### **3. What are the main implementations of the List interface?**

* ArrayList
* LinkedList
* Vector
* Stack

---

### **4. What is the difference between ArrayList and LinkedList?**

| Feature       | ArrayList     | LinkedList          |
| ------------- | ------------- | ------------------- |
| Storage       | Dynamic Array | Doubly Linked Nodes |
| Access        | Fast (O(1))   | Slow (O(n))         |
| Insert/Delete | Slow          | Fast                |
| Memory        | Less          | More                |

---

### **5. How does ArrayList grow internally?**

When ArrayList becomes full:

```
newCapacity = oldCapacity * 1.5
```

A new array is created and elements are copied into it.

---

### **6. What is the default capacity of an ArrayList?**

Default capacity = **10**
(Java 8+ creates array lazily after first element is added.)

---

### **7. Why is ArrayList good for searching but bad for insertion in the middle?**

* Searching (get by index) → **O(1)**
* Insertion in middle → Shifting required → **O(n)**

---

### **8. What is the advantage of LinkedList over ArrayList?**

LinkedList is better for:

* Frequent insertions
* Frequent deletions
  because it uses **nodes**, not array shifting.

---

### **9. Is LinkedList singly or doubly linked? Explain.**

LinkedList in Java is a **doubly linked list**.
Each node has:

```
prev pointer <--- data ---> next pointer
```

---

### **10. What is Vector? How is it different from ArrayList?**

Vector is the same as ArrayList but **synchronized**.
It is thread-safe but slower.

---

### **11. Is Vector synchronized? What does that mean?**

Yes.
Only **one thread** can access vector methods at a time, making it thread-safe.

---

### **12. Why is Vector slower than ArrayList?**

Because Vector methods are **synchronized**, causing more waiting time for threads.

---

### **13. What is Stack and what principle does it follow?**

Stack is a subclass of Vector and works on **LIFO (Last In First Out)**.

---

### **14. Name the commonly used Stack methods and their purpose.**

* `push()` → Add element
* `pop()` → Remove top element
* `peek()` → View top element
* `search()` → Find index of element

---

### **15. What are bulk operations in List? Explain addAll(), removeAll(), retainAll().**

| Method        | Purpose                                               |
| ------------- | ----------------------------------------------------- |
| `addAll()`    | Adds all elements from another collection             |
| `removeAll()` | Removes all matching elements from another collection |
| `retainAll()` | Keeps only the common (intersection) elements         |

---

---

## 🔗 Connect With Me

🌐 **Channel Name:** Safan Codes  

📺 **YouTube:**  
👉 https://www.youtube.com/@safanahmed9380  

💼 **LinkedIn:**  
👉 https://www.linkedin.com/in/safan-ahmed-515188251/  

---

