# CommerceCore API

CommerceCore is a **secure, production-style e-commerce backend** built with Spring Boot and Spring Security.  
It provides REST APIs for managing products, shopping carts, orders, and users, with **JWT-based authentication** and **role-based authorization**.

The project focuses on backend architecture, security, and real-world business logic rather than UI concerns.

---

## 🚀 Core Features

### Product & Category Management
- CRUD APIs for products, categories, and product images.
- Clear entity relationships and validation.
- Inventory tracking to prevent overselling.

### Shopping Cart
- Persistent user shopping carts.
- Add, update, remove, and clear cart items.
- Automatic cart total calculation and validation.

### Order Management
- Convert cart items into orders.
- Order and order-item persistence.
- Order history per user with status tracking.

### User & Role Management
- User registration and authentication.
- Role-based access control (Admin vs User).
- Secure password encoding and validation.

---

## 🔐 Security

- Stateless authentication using **JWT**.
- Spring Security filters for token validation.
- Method-level authorization for protected endpoints.
- Admin-only access to sensitive operations.

---

## 🧠 Architecture Overview

### Layered Design
- **Controller Layer** – REST endpoints and request validation.
- **Service Layer** – Business logic and domain rules.
- **Repository Layer** – Data persistence using JPA.
- **DTO Layer** – Clean API responses and decoupling from entities.

### Exception Handling
- Centralized global exception handling.
- Custom exceptions for common failure scenarios.
- Consistent and user-friendly error responses.

---

## 🛠 Tech Stack

- **Language**: Java  
- **Framework**: Spring Boot  
- **Security**: Spring Security, JWT  
- **Persistence**: JPA (Hibernate)  
- **Database**: MySQL  
- **Build Tool**: Maven  

---

## 🎯 Design Goals

- **Security First** – Stateless APIs suitable for horizontal scaling.
- **Separation of Concerns** – Clear architectural boundaries.
- **Maintainability** – DTO-based API design and modular services.
- **Extensibility** – Easy to add payments, shipping, or frontend clients.

---

## ⚠️ Tradeoffs & Limitations

- Backend-only service (no frontend included).
- Inventory logic is intentionally minimal.
- Payment and shipping integrations are out of scope.

These decisions were made to keep the core backend clean and extensible.

---

## 📈 What I Learned

- Designing secure REST APIs with **Spring Security + JWT**.
- Implementing role-based authorization in real-world workflows.
- Modeling complex entity relationships with JPA.
- Structuring maintainable backend services for production use.

---

## ▶️ Getting Started

1. Configure database credentials in `application.properties`.
2. Run the application:

```bash
./mvnw spring-boot:run
