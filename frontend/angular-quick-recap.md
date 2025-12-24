
#  Angular 20 –
---

## 1️⃣ What is Frontend?

Frontend is the **visible part of an application** that users interact with in the browser.

Examples:
- Input fields
- Buttons
- Pages
- Navigation menus

Angular is a **frontend framework** that helps us build this UI in a **structured and maintainable way**.

---

## 2️⃣ What is Angular?

Angular is a **TypeScript-based frontend framework** developed by **Google**.

### Why Angular?
We use Angular because:
- It breaks UI into **components**
- It handles **routing** (page navigation)
- It supports **forms**
- It makes **API calls**
- It provides **security (guards)**

Angular is commonly used for **enterprise-level applications**.

---

## 3️⃣ What is SPA (Single Page Application)?

In Angular, we build **Single Page Applications (SPA)**.

### Traditional Website:
- Every click loads a new HTML page from server
- Slow and inefficient

### SPA:
- Only one HTML page loads (`index.html`)
- Content changes dynamically
- No full page reload
- Faster user experience

Angular manages **view switching internally**, not by server.

---

## 4️⃣ What is Standalone Application in Angular?

Earlier Angular versions required **NgModules**.

From Angular 15+, we use **Standalone Components**.

### Why Standalone?
- Less boilerplate code
- Easy to understand
- Faster development
- Better for beginners

In Angular 20, **standalone is default**.

---

## 5️⃣ Angular Project Setup (How we start)

We use **Angular CLI** to create and manage projects.

### Why CLI?
- Creates correct structure
- Handles build & server
- Saves time

After creation, Angular runs on:
```

[http://localhost:4200](http://localhost:4200)

```

---

## 6️⃣ How Angular Application Starts (Bootstrap Flow)

Angular starts from:
1. `main.ts`
2. Bootstraps root component
3. Loads `index.html`
4. Renders component UI

This root component controls the **entire application UI**.

---

## 7️⃣ What is a Component?

Component is the **heart of Angular**.

### Why we use components?
- Split UI into small reusable pieces
- Easy maintenance
- Clear responsibility

### What a component contains:
- **Logic** (TypeScript)
- **View** (HTML)
- **Style** (CSS)

Each screen in Angular is a **component**.

---

## 8️⃣ How Data Appears in HTML (Interpolation)

Angular allows us to display data from TypeScript into HTML.

### Why?
HTML alone cannot access variables.

### How?
Using **interpolation syntax**:

```

{{ variableName }}

```

This is used **only for display purposes**.

---

## 9️⃣ What is Data Binding?

Data binding connects:
- Component data
- HTML view

### Why data binding?
- Sync UI with data
- Reduce manual DOM updates

### Types:
- One-way (display or event)
- Two-way (input ↔ component)

---

## 🔟 Why and How We Use ngModel (Two-Way Binding)

### Problem:
User enters data, but component doesn't know it.

### Solution:
We use `ngModel`.

### Why ngModel?
- Automatically sync input value with component variable
- Keeps data updated in both directions

### Where used?
- Forms
- Input fields

`ngModel` belongs to **FormsModule**.

---

## 1️⃣1️⃣ What are Directives?

Directives are used to **change DOM behavior**.

### Why?
HTML is static. Directives make it dynamic.

### Common Use:
- Show/hide elements
- Loop through data
- Apply styles conditionally

---

## 1️⃣2️⃣ Structural Directives (*ngIf, *ngFor)

### Why *ngIf?
To conditionally show or hide UI.

### Why *ngFor?
To repeat elements dynamically.

Angular modifies the DOM based on data values.

---

## 1️⃣3️⃣ Component Lifecycle (How a Component Lives)

Each component has a **lifecycle**.

### Why lifecycle hooks?
To run code at specific times:
- When component loads
- When data changes
- When component is destroyed

Most commonly used:
- `ngOnInit()` → component initialization

---

## 1️⃣4️⃣ What is Routing?

Routing allows us to **navigate between pages** without reloading.

### Why routing?
- Multiple pages in SPA
- Clean URLs
- Better UX

Angular routing is **client-side routing**.

---

## 1️⃣5️⃣ How Routing Works Internally

### Step 1: Route Configuration
We define **path → component mapping**.

This tells Angular:
> When URL is `/dashboard`, show Dashboard component

This configuration is done in **route configuration file**.

---

### Step 2: Router Outlet

`router-outlet` acts as a **placeholder**.

### Why router-outlet?
Angular needs a place to **display routed components**.

Without `router-outlet`, routes will not display.

---

## 1️⃣6️⃣ What is a Service?

Service is a **class that contains reusable logic**.

### Why services?
- Share data between components
- Keep components clean
- Handle business logic
- Handle authentication

Services are **not UI elements**.

---

## 1️⃣7️⃣ What is Dependency Injection (DI)?

DI means Angular **creates and provides services automatically**.

### Why DI?
- No manual object creation
- Loose coupling
- Easy testing

Angular injects services through **constructor**.

---

## 1️⃣8️⃣ What is HttpClient?

Angular uses `HttpClient` to **communicate with backend servers**.

### Why HttpClient?
- Fetch data
- Send data
- Update data
- Delete data

All HTTP calls return **Observables**.

---

## 1️⃣9️⃣ What is RxJS & Observable?

Observable represents **future data**.

### Why Observables?
- Handle async operations
- Cancel requests
- Multiple values over time

Angular heavily relies on RxJS.

---

## 2️⃣0️⃣ What are Pipes?

Pipes transform **data before displaying it**.

### Why pipes?
- Clean HTML
- Reusable formatting
- Better performance

Pipes do **not modify original data**.

---

## 2️⃣1️⃣ What are Forms?

Forms collect **user input**.

Examples:
- Login
- Registration
- Feedback

Angular provides two types of forms.

---

## 2️⃣2️⃣ Template-Driven Forms (Why & How)

### Why Template-Driven Forms?
- Simple
- Easy for beginners
- Less TypeScript

Form logic is written mainly in **HTML**.

Angular automatically tracks:
- Validity
- Errors
- User interaction

---

## 2️⃣3️⃣ What is Authentication Flow?

Authentication controls **who can access what**.

### Flow:
1. User logs in
2. Token generated
3. Token stored in browser
4. Token checked before navigation

This is frontend-level security.

---

## 2️⃣4️⃣ Why Local Storage?

Local Storage is used to:
- Store token
- Persist login
- Avoid re-login on refresh

Data remains even after browser refresh.

---

## 2️⃣5️⃣ What are Route Guards?

Guards control **route access**.

### Why guards?
- Protect pages
- Restrict unauthorized users
- Role-based access

Guard runs **before route loads**.

---

## 2️⃣6️⃣ Logout Flow

Logout simply means:
- Remove token
- Redirect to login
- Block protected routes

---

## 2️⃣7️⃣ Component Communication

### Why communication?
Components do not share data by default.

### Common scenarios:
- Parent sends data to child
- Child notifies parent

Angular provides decorators for this.

---

## 2️⃣8️⃣ Final Big Picture 

1. Angular loads root component
2. Routing controls page navigation
3. Components render UI
4. Services handle logic
5. Forms collect input
6. Guards protect routes
7. Pipes format data

---


