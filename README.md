# MHMart - Technology Stack & Architecture Specification

## 1. Project Overview
* **Project Name:** MHMart
* **Architecture Pattern:** Decoupled Client-Server (Single-Page Application frontend with RESTful Web API backend)
* **Development OS:** Ubuntu Linux
* **Reference Model:** E-commerce grocery/essentials shop 

---

## 2. Frontend Stack

| Layer / Concern | Technology | Purpose & Context |
| :--- | :--- | :--- |
| **Runtime / Library** | React (v18+) | Declarative, component-based UI library to render dynamic interfaces. |
| **Build Tool & Bundler** | Vite | Next-generation frontend tooling providing near-instant dev server startup and Hot Module Replacement (HMR). |
| **Styling** | Tailwind CSS | Utility-first CSS framework for responsive, design-system-aligned styling without custom `.css` bloat. |
| **Client-Side Routing** | React Router DOM | Handles navigation and route switching (`/`, `/product/:id`, `/checkout`, `/admin`) without full browser reloads. |
| **State Management** | Zustand | Lightweight global store for managing the shopping cart state, user session, and slide-out drawer visibility across all components. |
| **HTTP Client** | Axios / Fetch API | Executes asynchronous HTTP requests to interact with the ASP.NET Core backend endpoints. |
| **Icons & UI Assets** | Lucide React | Clean, tree-shakeable SVG icons for carts, search bars, user profiles, and badges. |
| **Notifications** | React Hot Toast | Lightweight toast notifications for user actions (e.g., "Added to cart", "Order confirmed"). |

---

## 3. Backend Stack

| Layer / Concern | Technology | Purpose & Context |
| :--- | :--- | :--- |
| **Framework / Runtime** | .NET 8 / ASP.NET Core Web API | High-performance, cross-platform runtime and API framework running natively on Ubuntu. |
| **Language** | C# (C-Sharp) | Strongly typed, object-oriented language ensuring type safety and robust business logic. |
| **API Architecture** | RESTful Controllers | Structured controllers (`ProductsController`, `OrdersController`, `AuthController`) exposing standard HTTP verbs (`GET`, `POST`, `PUT`, `DELETE`). |
| **Database** | PostgreSQL | Open-source relational database management system (RDBMS) for persistent data storage. |
| **ORM** | Entity Framework Core (EF Core 8) | Object-Relational Mapper to query, insert, and update PostgreSQL tables using native C# classes and LINQ. |
| **Database Driver** | Npgsql.EntityFrameworkCore.PostgreSQL | PostgreSQL provider for Entity Framework Core. |
| **Authentication & Auth** | JWT (JSON Web Tokens) & BCrypt.Net | Token-based stateless authentication using `System.IdentityModel.Tokens.Jwt` and secure password hashing. |
| **API Documentation** | Swagger / OpenAPI (Swashbuckle) | Interactive documentation UI available at `/swagger` to test endpoints during development. |

---

## 4. Repository Structure (Monorepo Layout)

```text
MHMart/
├── frontend/                     # React Single-Page Application (Vite)
│   ├── public/                   # Static assets (favicons, public images)
│   ├── src/
│   │   ├── assets/               # Brand logos and local vector files
│   │   ├── components/           # Reusable UI (Navbar, ProductCard, CartDrawer)
│   │   ├── pages/                # Route views (Home, Checkout, ProductDetails, Admin)
│   │   ├── services/             # API layer and endpoint caller functions
│   │   ├── store/                # Zustand global stores (useCartStore, useAuthStore)
│   │   ├── App.jsx               # Root component with routing configuration
│   │   └── main.jsx              # React DOM mounting entry point
│   ├── index.html                # Single HTML entry file
│   ├── package.json              # NPM dependencies and scripts
│   ├── tailwind.config.js        # Tailwind design tokens (colors, fonts)
│   └── vite.config.js            # Vite build configuration
│
├── backend/                      # ASP.NET Core Web API Project
│   ├── Controllers/              # HTTP request entry points
│   │   ├── AuthController.cs
│   │   ├── ProductsController.cs
│   │   └── OrdersController.cs
│   ├── Data/                     # Database access layer
│   │   ├── ApplicationDbContext.cs
│   │   └── DbInitializer.cs      # Product seeding script
│   ├── DTOs/                     # Data Transfer Objects (contracts sent over network)
│   │   ├── ProductDto.cs
│   │   └── OrderCreateDto.cs
│   ├── Models/                   # Domain entities mapped to PostgreSQL tables
│   │   ├── Product.cs
│   │   ├── Order.cs
│   │   ├── OrderItem.cs
│   │   └── User.cs
│   ├── Migrations/               # EF Core database version history
│   ├── appsettings.json          # Production configuration & keys
│   ├── appsettings.Development.json # Local connection strings
│   ├── backend.csproj            # NuGet package references
│   └── Program.cs                # Dependency injection, middleware & routing setup
│
├── .gitignore                    # Git exclusion rules (node_modules, bin, obj)
└── TECH_STACK.md                 # System architecture and stack reference
```

---

## 5. End-to-End Data Flow

```text
+-----------------------------------------------------------+
|                      CLIENT (Browser)                     |
|                                                           |
|   React Components  <--->  Zustand Store (Cart & Auth)    |
|          │                                                |
|          ▼                                                |
|   HTTP Calls (Axios / Fetch)                              |
+--------------------------│--------------------------------+
                           │ JSON over HTTP (Port 5xxx)
                           ▼
+-----------------------------------------------------------+
|                    SERVER (ASP.NET Core)                  |
|                                                           |
|   Middleware Pipeline (CORS -> Auth JWT Validation)       |
|          │                                                |
|          ▼                                                |
|   Controllers (ProductsController / OrdersController)     |
|          │                                                |
|          ▼ (C# Entities / LINQ)                           |
|   Entity Framework Core (ApplicationDbContext)            |
+--------------------------│--------------------------------+
                           │ SQL Queries (Npgsql Driver)
                           ▼
+-----------------------------------------------------------+
|                     DATABASE (PostgreSQL)                 |
|                                                           |
|   Tables: Products | Orders | OrderItems | Users          |
+-----------------------------------------------------------+
```

---

## 6. Development Tools & Prerequisites (Ubuntu)

* **Runtime:** .NET 8.0 SDK (`dotnet --version`)
* **Package Manager:** Node.js v20+ LTS & npm (`node -v`, `npm -v`)
* **Database Service:** PostgreSQL 14+ (`sudo systemctl status postgresql`)
* **Code Editor:** Visual Studio Code with:
  * C# Dev Kit (by Microsoft)
  * Tailwind CSS IntelliSense
  * ES7+ React/Redux/React-Native snippets