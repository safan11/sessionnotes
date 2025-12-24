

# 🔐 PROTECTED ROUTES IN ANGULAR 20 

## 1️⃣ What Changed in Angular 17–20 (IMPORTANT)

### Old Angular (Before)

* `app.module.ts`
* NgModules
* `declarations`, `imports`

### Angular 17–20 (Now)

✅ **Standalone Components**
✅ No `AppModule`
✅ Direct imports inside components
✅ Simpler & faster

👉 **Everything is a component now**

---

## 2️⃣ What You Will Build

| Feature          | Description                    |
| ---------------- | ------------------------------ |
| Login            | Dummy login page               |
| Token            | Stored in browser localStorage |
| Header           | Shows navigation + logout      |
| Protected Routes | Dashboard & Admin              |
| Role Guard       | Admin-only access              |
| Logout           | Clears token                   |

---

## 3️⃣ Create Angular 20 Project (Standalone)

```bash
ng new angular20-protected-routes
```

Choose:

* Routing → **YES**
* Styles → **CSS**
* Standalone → **YES (default)**

Run project:

```bash
cd angular20-protected-routes
ng serve
```

Open:

```
http://localhost:4200
```

---

## 4️⃣ Generate Components (Angular 20 Way)

```bash
ng generate component header
ng generate component login
ng generate component dashboard
ng generate component admin
```

---

## 5️⃣ Correct Folder Structure (Angular 20)

```text
src/
 ├── app/
 │   ├── header/
 │   │   ├── header.ts
 │   │   ├── header.html
 │   │   └── header.css
 │
 │   ├── login/
 │   │   ├── login.ts
 │   │   ├── login.html
 │   │   └── login.css
 │
 │   ├── dashboard/
 │   │   ├── dashboard.ts
 │   │   ├── dashboard.html
 │   │   └── dashboard.css
 │
 │   ├── admin/
 │   │   ├── admin.ts
 │   │   ├── admin.html
 │   │   └── admin.css
 │
 │   ├── guards/
 │   │   ├── auth.guard.ts
 │   │   └── admin.guard.ts
 │
 │   ├── services/
 │   │   └── auth.service.ts
 │
 │   ├── app.ts
 │   ├── app.html
 │   └── app.routes.ts
 │
 ├── main.ts
 └── index.html
```

📌 **NO `app.module.ts`**

---

## 6️⃣ Dummy Users (No Backend)

| Username | Password | Role  |
| -------- | -------- | ----- |
| user     | user123  | USER  |
| admin    | admin123 | ADMIN |

---

## 7️⃣ Authentication Service

📁 `src/app/services/auth.service.ts`

```ts
import { Injectable } from '@angular/core';
import { Router } from '@angular/router';

@Injectable({ providedIn: 'root' })
export class AuthService {

  constructor(private router: Router) {}

  private users = [
    { username: 'user', password: 'user123', role: 'USER' },
    { username: 'admin', password: 'admin123', role: 'ADMIN' }
  ];

  login(username: string, password: string): boolean {
    const user = this.users.find(
      u => u.username === username && u.password === password
    );

    if (user) {
      localStorage.setItem('token', 'fake-jwt-token');
      localStorage.setItem('role', user.role);
      return true;
    }
    return false;
  }

  logout() {
    localStorage.clear();
    this.router.navigate(['/login']);
  }

  isLoggedIn(): boolean {
    return !!localStorage.getItem('token');
  }

  getRole(): string | null {
    return localStorage.getItem('role');
  }
}
```

---

## 8️⃣ Auth Guard (Protected Routes)

📁 `src/app/guards/auth.guard.ts`

```ts
import { CanActivateFn, Router } from '@angular/router';
import { inject } from '@angular/core';
import { AuthService } from '../services/auth.service';

export const authGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  const router = inject(Router);

  if (auth.isLoggedIn()) {
    return true;
  }

  router.navigate(['/login']);
  return false;
};
```

📌 Angular 20 uses **functional guards**

---

## 9️⃣ Admin Guard (Role-Based)

📁 `src/app/guards/admin.guard.ts`

```ts
import { CanActivateFn, Router } from '@angular/router';
import { inject } from '@angular/core';
import { AuthService } from '../services/auth.service';

export const adminGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  const router = inject(Router);

  if (auth.getRole() === 'ADMIN') {
    return true;
  }

  router.navigate(['/dashboard']);
  return false;
};
```

---

## 🔟 Routing Configuration (Standalone)

📁 `src/app/app.routes.ts`

```ts
import { Routes } from '@angular/router';
import { LoginComponent } from './login/login';
import { DashboardComponent } from './dashboard/dashboard';
import { AdminComponent } from './admin/admin';
import { authGuard } from './guards/auth.guard';
import { adminGuard } from './guards/admin.guard';

export const routes: Routes = [
  { path: '', redirectTo: 'login', pathMatch: 'full' },

  { path: 'login', component: LoginComponent },

  {
    path: 'dashboard',
    component: DashboardComponent,
    canActivate: [authGuard]
  },

  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [authGuard, adminGuard]
  }
];
```

---

## 1️⃣1️⃣ App Component (Standalone Root)

📁 `src/app/app.ts`

```ts
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { HeaderComponent } from './header/header';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, HeaderComponent],
  templateUrl: './app.html'
})
export class AppComponent {}
```

📁 `src/app/app.html`

```html
<app-header></app-header>
<hr />
<router-outlet></router-outlet>
```

---

## 1️⃣2️⃣ Header Component (Navigation + Logout)

📁 `header.ts`

```ts
import { Component } from '@angular/core';
import { RouterLink } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Component({
  selector: 'app-header',
  standalone: true,
  imports: [RouterLink],
  templateUrl: './header.html'
})
export class HeaderComponent {

  constructor(public auth: AuthService) {}

  logout() {
    this.auth.logout();
  }
}
```

📁 `header.html`

```html
<nav>
  <a routerLink="/login">Login</a> |
  <a routerLink="/dashboard">Dashboard</a> |
  <a routerLink="/admin">Admin</a> |
  <button (click)="logout()">Logout</button>
</nav>
```

---

## 1️⃣3️⃣ Login Component (Standalone)

📁 `login.ts`

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Component({
  standalone: true,
  selector: 'app-login',
  imports: [FormsModule],
  templateUrl: './login.html'
})
export class LoginComponent {

  username = '';
  password = '';
  error = '';

  constructor(
    private auth: AuthService,
    private router: Router
  ) {}

  login() {
    if (this.auth.login(this.username, this.password)) {
      this.router.navigate(['/dashboard']);
    } else {
      this.error = 'Invalid credentials';
    }
  }
}
```

📁 `login.html`

```html
<h3>Login</h3>

<input [(ngModel)]="username" placeholder="Username" />
<br /><br />

<input type="password" [(ngModel)]="password" placeholder="Password" />
<br /><br />

<button (click)="login()">Login</button>

<p style="color:red">{{ error }}</p>
<p>user / user123 | admin / admin123</p>
```

---

## 1️⃣4️⃣ Dashboard Component

📁 `dashboard.ts`

```ts
import { Component } from '@angular/core';

@Component({
  standalone: true,
  templateUrl: './dashboard.html'
})
export class DashboardComponent {}
```

📁 `dashboard.html`

```html
<h3>Dashboard (Protected)</h3>
<p>You are logged in</p>
```

---

## 1️⃣5️⃣ Admin Component

📁 `admin.ts`

```ts
import { Component } from '@angular/core';

@Component({
  standalone: true,
  templateUrl: './admin.html'
})
export class AdminComponent {}
```

📁 `admin.html`

```html
<h3>Admin Page</h3>
<p>Only ADMIN users can access this</p>
```

---

## 1️⃣6️⃣ How to See Token in Browser 🔍

1. Open Chrome
2. Right-click → Inspect
3. Application → Local Storage
4. Click `http://localhost:4200`

You’ll see:

```
token → fake-jwt-token
role → USER / ADMIN
```

---

##  KEY TAKEAWAYS 

* Angular 20 uses **standalone by default**
* Guards are **functions**, not classes
* No `AppModule`
* Token = login proof
* Role = authorization
* Same pattern used in real production apps


---

# 🔄 COMPLETE FLOW EXPLANATION

## Angular 20 – Protected Routes 

I’ll explain this in **real-life language**, then map it to **actual files**, then show **browser behavior**.

---

## 1️⃣ Application Startup Flow 

### What happens when you run:

```bash
ng serve
```

### Step-by-step:

```
Browser → index.html
        ↓
main.ts
        ↓
AppComponent (app.ts)
        ↓
RouterOutlet
```

### Explanation:

1. **Browser loads `index.html`**
2. Angular bootstraps the app from `main.ts`
3. `AppComponent` becomes the **root component**
4. `<router-outlet>` waits for route-based components

📌 Think of `<router-outlet>` as:

> “Angular, load the page component here”

---

## 2️⃣ Root Layout Flow (`app.ts` + `app.html`)

### `app.html`

```html
<app-header></app-header>
<hr />
<router-outlet></router-outlet>
```

### What this means:

* `HeaderComponent` is **always visible**
* Page content changes inside `router-outlet`

```
Header (static)
----------------
Page (dynamic)
```

---

## 3️⃣ User Opens the App First Time

### URL:

```
http://localhost:4200
```

### Router checks `app.routes.ts`

```ts
{ path: '', redirectTo: 'login', pathMatch: 'full' }
```

### Result:

```
User → redirected to /login
```

📌 No guard runs here because login is **public**

---

## 4️⃣ Login Page Flow

### User sees:

* Username field
* Password field
* Login button

### User clicks **Login**

```ts
login() {
  if (this.auth.login(username, password)) {
    router.navigate(['/dashboard']);
  }
}
```

---

## 5️⃣ What Happens Inside `AuthService.login()`

📁 `auth.service.ts`

```
User enters credentials
        ↓
Angular checks dummy users list
        ↓
If match found:
        ↓
Store token in localStorage
Store role in localStorage
        ↓
Return TRUE
```

### Browser Storage After Login:

```
localStorage
 ├── token = "fake-jwt-token"
 └── role = "ADMIN" / "USER"
```

📌 **Token = proof of login**
📌 **Role = permission level**

---

## 6️⃣ Navigation to Dashboard (IMPORTANT PART)

After successful login:

```
router.navigate(['/dashboard'])
```

Angular does NOT load the page immediately.

Instead 👇

---

## 7️⃣ Route Guard Execution Flow

### Route definition:

```ts
{
  path: 'dashboard',
  component: DashboardComponent,
  canActivate: [authGuard]
}
```

### Actual flow:

```
Navigation Request
        ↓
authGuard runs
        ↓
auth.isLoggedIn()
        ↓
Check localStorage token
```

### Case 1: Token exists ✅

```
Guard returns true
→ DashboardComponent loads
```

### Case 2: Token missing ❌

```
Guard redirects to /login
→ Dashboard blocked
```

📌 **Guards ALWAYS run before component loads**

---

## 8️⃣ Admin Route Flow (Role-Based Protection)

### Admin route:

```ts
{
  path: 'admin',
  component: AdminComponent,
  canActivate: [authGuard, adminGuard]
}
```

### Execution Order:

```
authGuard runs first
        ↓
adminGuard runs second
```

### Full flow:

```
Check token
   ↓
Check role
   ↓
Allow or redirect
```

### Example:

| User Role | Result                 |
| --------- | ---------------------- |
| USER      | Redirect to /dashboard |
| ADMIN     | Admin page allowed     |

---

## 9️⃣ What Happens on Page Refresh 🔄

User presses **F5 / Refresh**

```
Browser reloads
        ↓
Angular restarts
        ↓
localStorage still exists
```

📌 **localStorage survives refresh**

Guards re-check token → user stays logged in

---

## 🔟 Header Component Flow

### Why header works everywhere?

```ts
imports: [HeaderComponent]
```

Header is inside `app.html`, not route-based.

### Logout Flow:

```ts
logout() {
  auth.logout();
}
```

### AuthService.logout():

```
Clear localStorage
        ↓
Navigate to /login
        ↓
Guards block protected pages
```

---

## 1️⃣1️⃣ What Happens If User Manually Types URL?

### User types:

```
/dashboard
```

### Angular flow:

```
URL change detected
        ↓
authGuard runs
        ↓
Token?
```

### No token ❌

```
Redirect to /login
```

📌 **Manual URL typing cannot bypass guards**

---

## 1️⃣2️⃣ Browser Token Inspection Flow

Steps:

```
Right click → Inspect
        ↓
Application
        ↓
Local Storage
        ↓
http://localhost:4200
```

You see:

```
token = fake-jwt-token
role = ADMIN
```

Delete token manually → refresh → redirected to login

---

## 1️⃣3️⃣ COMPLETE FLOW DIAGRAM (TEXT)

```
App Load
   ↓
Login Page
   ↓
User Login
   ↓
Token Stored
   ↓
Navigate to Dashboard
   ↓
AuthGuard checks token
   ↓
AdminGuard checks role (if admin route)
   ↓
Component Loads
```

---

## 🔑 KEY CONCEPTS YOU JUST MASTERED

| Concept        | Meaning                |
| -------------- | ---------------------- |
| Authentication | Who you are            |
| Authorization  | What you can access    |
| Guard          | Route protection logic |
| Token          | Login proof            |
| Role           | Permission level       |
| Standalone     | No AppModule           |

---

## ❌ Common Beginner Confusions (Cleared)

❌ “Why page doesn’t load?”
✔ Guard blocked it

❌ “Why refresh doesn’t logout?”
✔ localStorage persists

❌ “Why admin page blocked?”
✔ Role is USER

---

## 🎯 REAL-WORLD CONNECTION

This same flow is used in:

* Banking apps
* Corporate dashboards
* Admin panels
* E-commerce admin systems

Only difference:
👉 Token comes from backend (JWT)

---

## ✅ FINAL TAKEAWAY

> **Angular routing is NOT magic.**
> It is a **series of checks before page loading**.

Once you understand this flow, **90% of Angular confusion disappears**.

---



