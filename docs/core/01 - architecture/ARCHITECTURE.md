# Amber-Ale Architecture Overview

## 🎯 **Core Architectural Goals**

Amber-Ale is a **modular, open-source, multi-tenant platform** designed to provide **dynamic module loading, database extensibility, and customizable UI**. The architecture enables users to create **independent tenant instances**, install **TypeScript/JavaScript modules dynamically**, and customize the **Vue 3 UI without code modifications**.

---

## 🏗 **High-Level Architecture**

Amber-Ale consists of three primary layers:

1. **Backend** – A **.NET 8/9-based API** that manages tenants, modules, and database extensions.
2. **Frontend** – A **Vue 3-based UI framework** that supports dynamic component registration and customization.
3. \*\*Module System – A TypeScript-based module system where modules are built externally following documented guidelines. Developers will create modules independently and register them within Amber-Ale using a `moduleinfo.json` file, which provides the necessary metadata for proper installation, registration, and integration into the core platform. This allows for dynamic feature addition without requiring an app restart, \*\*

### **ASCII Diagram: High-Level Architecture**

```
+-----------------------+
|    Amber-Ale Core    |
|  (.NET 8/9 Backend)  |
+-----------------------+
          |
          v
+-------------------------------+
|       Multi-Tenant System     |
| (Database Siloing Per Tenant) |
+-------------------------------+
          |
          v
+-------------------------------+
|        Module System          |
| (Dynamic TypeScript Modules)  |
+-------------------------------+
          |
          v
+-------------------------------+
|       Vue 3 Frontend          |
| (Dynamic UI + Theming)        |
+-------------------------------+
```

---

## 🔌 **Multi-Tenant System & Database Extensibility**

Amber-Ale supports **multi-tenancy**, ensuring data is **siloed per instance** while allowing **extensible database structures** for modular customization.

### **🔹 Multi-Tenant Isolation**

- Each tenant has **its own schema** in the database (PostgreSQL, MySQL, or MSSQL).
- A super admin can manage multiple tenants but **each tenant is isolated**.
- Modules **only affect the tenants they are installed for**.

### **🔹 Database Extension API**

- A reserved **core schema** for platform-critical data.
- Modules can register **new tables, modify existing ones, and store their own data**.
- A **safe API layer** prevents breaking core functionality while allowing extensibility.

### **ASCII Diagram: Multi-Tenant System**

```
+-----------------------------+
|   Super Admin Interface    |
|  (Manages Tenants & Mods)  |
+-----------------------------+
          |
          v
+-----------------------------+
|      Tenant 1 (Schema 1)    |
|  Modules: A, B, C          |
+-----------------------------+
          |
          v
+-----------------------------+
|      Tenant 2 (Schema 2)    |
|  Modules: B, D             |
+-----------------------------+
```

---

## 🔑 **User Management & Super Admin Control**

Amber-Ale includes **basic user management** focused on **super admins**:

- **Super Admins** – Users with **outside access** who manage the **tenant, enable/disable modules, and configure system settings**.
- **No Built-in Tenant User Management** – Full user management within a tenant (e.g., roles, permissions) is handled **by modules**, not the core system.

This ensures that **each instance defines its own user structure** through its enabled modules.

---

## 🚀 **Module System (TypeScript/JavaScript-Based)**

Amber-Ale’s **module system dynamically loads TypeScript modules** per tenant without requiring a restart.

### **🔹 Module Registry & Dynamic Loading**

- Modules are registered via a **module registry**, which stores metadata.
- Modules are **loaded dynamically while the application is running**, allowing for installation, enabling, and disabling without requiring a restart.
- Modules can **extend API functionality, register UI components, modify the database, and register Pub/Sub events for communication between modules**.

### **🔹 Module Capabilities**

- **Custom API Endpoints** – Modules can expose API routes via the backend.
- **Custom Event Hooks** – Modules can register with the **Pub/Sub system**.
- **UI Extensions** – Modules can dynamically **inject Vue components**.
- **Tenant-Specific Activation** – Modules are scoped to the **tenant they are enabled for**.
- **Core Component Library** – Modules can utilize a **Vue 3 UI library** to maintain consistent design and theme.

### **ASCII Diagram: Module System**

```
+--------------------------------+
|        Module Registry        |
|  (Tracks Installed Modules)   |
+--------------------------------+
          |
          v
+--------------------------------+
|    TypeScript Modules         |
| (APIs, UI Components, Events, |
|   Pub/Sub Event System)       |
+--------------------------------+
          |
          v
+--------------------------------+
|   Vue 3 Component Library     |
| (Reusable Themed UI Elements) |
+--------------------------------+
```

---

## 🎨 **Vue 3-Based Frontend & Dynamic UI Customization**

Amber-Ale’s frontend is built with **Vue 3** and is designed for **dynamic theming and module integration**.

### **🔹 Dynamic Component Registration**

- Modules register UI components dynamically without recompiling.
- A **Vue component registry** loads components based on active modules.
- Components are **lazy-loaded to optimize performance**.
- A **Core Component Library** provides **pre-built UI elements** that maintain a consistent design across modules.

### **🔹 Theming & UI Customization**

- A **backend-driven settings panel** allows customization of:
  - **Colors & Fonts**
  - **Layout & Styling**
  - **UI Components**
- Themes and UI changes **apply dynamically without modifying the core Vue templates**.

### **🔹 Marketplace & Module Management**

- A built-in **Marketplace UI** lets users browse, install, and configure modules.
- Modules define their **own UI extensions**, appearing dynamically in the platform.
- Modules can leverage **core UI components** to maintain a unified experience.

### **ASCII Diagram: Vue 3 UI**

```
+-------------------------------+
|  Vue 3 Component Registry    |
| (Loads UI Elements from Mods |
|   & Handles Pub/Sub Events)  |
+-------------------------------+
          |
          v
+-------------------------------+
|  Core Component Library      |
| (Reusable, Themed UI)        |
+-------------------------------+
          |
          v
+-------------------------------+
|  User Customization Panel    |
| (Themes, Layouts, Styling)   |
+-------------------------------+
```

---

## 📌 **Summary**

✅ **Multi-Tenant System** – Ensures isolated, extensible data per tenant.\
✅ **Basic Super Admin Control** – Super admins manage tenant settings, but user management is left to modules.\
✅ **Dynamic Modules** – Allows TypeScript/JavaScript modules to register new APIs, UI components, and database extensions **without a restart**.\
✅ **Vue 3 UI Customization** – Enables users to modify themes, layouts, and interface elements dynamically.\
✅ **Extensible Marketplace** – Supports a growing module ecosystem, making the platform adaptable for **roleplay communities, businesses, and beyond**.\
✅ **Core UI Component Library** – Ensures design consistency across all modules.

---

## 🔮 **Next Steps**

- **Finalize the module API specifications.**
- **Define Vue 3 registry interactions for module-based UI extensions.**
- **Develop the tenant and database extension framework.**

