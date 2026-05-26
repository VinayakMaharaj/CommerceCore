# CommerceCore

A production-style e-commerce REST API built with Spring Boot 3.3.2 and Java 17. Covers the full backend surface of an e-commerce platform — products, categories, shopping carts, orders, users, and image uploads — with a clean layered architecture and interface-based service design throughout.

---

## API Overview

All endpoints are prefixed with `/api/v1`.

| Resource | Endpoints |
|---|---|
| **Products** | `GET /products/all` · `GET /product/{id}/product` · `POST /products/add` · `PUT /product/{id}/update` · `DELETE /product/{id}/delete` · filter by brand, category, name |
| **Categories** | `GET /categories/all` · `GET /category/{id}/category` · `POST /categories/add` · `PUT /category/{id}/update` · `DELETE /category/{id}/delete` |
| **Cart** | `GET /carts/{cartId}/my-cart` · `GET /carts/{cartId}/cart/total-price` · `DELETE /carts/{cartId}/clear` |
| **Cart Items** | `POST /cartItems/item/add` · `PUT /cart/{cartId}/item/{itemId}/update` · `DELETE /cart/{cartId}/item/{itemId}/remove` |
| **Orders** | `POST /orders/order` · `GET /orders/{orderId}/order` · `GET /orders/{userId}/order` |
| **Users** | `GET /users/{userId}/user` · `POST /users/add` · `PUT /users/{userId}/update` · `DELETE /users/{userId}/delete` |
| **Images** | `POST /images/upload` · `GET /images/image/download/{id}` · `PUT /images/image/{id}/update` · `DELETE /images/image/{id}/delete` |

---

## Architecture

**Layered design** with a strict separation between concerns. Every domain has an interface and a concrete implementation, keeping the controller layer thin and the business logic testable.

```
Controller  →  IService  →  ServiceImpl  →  Repository  →  Entity
                                        ↓
                                      ModelMapper
                                        ↓
                                       DTO
```

**Domains and their service interfaces:**
- `IProductService` / `ProductService`
- `ICategoryService` / `CategoryService`
- `ICartService` / `CartService`
- `ICartItemService` / `CartItemService`
- `IOrderService` / `OrderService`
- `IUserService` / `UserService`
- `IImageService` / `ImageService`

**Exception handling** uses three custom exceptions — `ResourceNotFoundException`, `AlreadyExistsException`, `ProductNotFoundException` — with consistent `ApiResponse` wrappers on every endpoint.

---

## Data Model

```
User ──────────── Cart ──────── CartItem ──── Product ──── Category
  │                                                  │
  └── Order ──── OrderItem ─────────────────────────┘
                                 │
                               Image
```

Key design decisions:
- Cart total recalculates on every `addItem` / `removeItem` via `updateTotalAmount()`
- `@Transactional` on `placeOrder` — converts cart to order and clears the cart atomically
- `@Transactional` on `clearCart` — deletes all cart items before removing the cart
- Inventory decrements at order placement to prevent overselling
- Images stored as `Blob` with a generated download URL per image

---

## Tech Stack

| | |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot 3.3.2 |
| Persistence | Spring Data JPA (Hibernate) |
| Database | MySQL |
| Mapping | ModelMapper |
| Boilerplate | Lombok |
| Build | Maven (Maven Wrapper included) |

---

## Project Structure

```
src/main/java/com/dailycodework/dreamshops/
├── controller/        # REST endpoints — thin, delegates to services
├── service/           # Interface + implementation per domain
├── repository/        # Spring Data JPA interfaces
├── model/             # JPA entities
├── dto/               # API response shapes (decoupled from entities)
├── request/           # Request body models
├── response/          # ApiResponse wrapper
├── exceptions/        # Custom runtime exceptions
├── enums/             # OrderStatus: PENDING, PROCESSING, SHIPPED, DELIVERED, CANCELLED
└── config/            # ModelMapper bean
```

---

## Getting Started

1. Create a MySQL database called `dream_shops_db`
2. Update credentials in `src/main/resources/application.properties`
3. Run:

```bash
./mvnw spring-boot:run
```

Server starts on port `9191`.

---

## Design Notes

This project intentionally has no frontend, no Spring Security configuration, and minimal inventory logic. The goal was to build a backend with clean architectural boundaries that could realistically accept those additions without restructuring. The service interfaces make it straightforward to swap implementations or add a test double. The DTO layer means API responses are stable regardless of internal entity changes.

