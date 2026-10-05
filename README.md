![Java](https://img.shields.io/badge/Java-17-orange?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.3-6DB33F?logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/Spring%20Data%20JPA-enabled-6DB33F?logo=spring&logoColor=white)
![Spring Data MongoDB](https://img.shields.io/badge/Spring%20Data%20MongoDB-enabled-47A248?logo=mongodb&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.x-4479A1?logo=mysql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-7.x-47A248?logo=mongodb&logoColor=white)
![Maven](https://img.shields.io/badge/Build-Maven-C71A36?logo=apachemaven&logoColor=white)
![License](https://img.shields.io/badge/License-See%20LICENSE-blue)

# Java Database Final – Store Management System

> A pedagogical Spring Boot application illustrating the integration of **relational (MySQL)** and **document-oriented (MongoDB)** databases within a single layered architecture, featuring a **store management system** with inventory control, order processing, stored procedures for sales reporting, and a frontend served directly from the backend.

## Table of Contents

- [Abstract](#abstract)
- [Overview](#overview)
- [Technologies and Dependencies](#technologies-and-dependencies)
- [Project Structure](#project-structure)
- [Architectural Pattern: Layered Architecture](#architectural-pattern-layered-architecture)
- [Data Model](#data-model)
- [⚙️ Service Layer](#-service-layer)
- [Controller Endpoints](#controller-endpoints)
- [Stored Procedures](#stored-procedures)
- [Database Host Configuration: Local vs. Remote](#database-host-configuration-local-vs-remote)
- [Building and Running the Application](#building-and-running-the-application)
- [Usage](#usage)
- [Frontend Interface](#frontend-interface)
- [Testing](#testing)
- [Limitations and Future Work](#limitations-and-future-work)
- [License](#license)
- [Conclusion](#conclusion)

## Abstract

This repository contains a comprehensive demonstration of **database integration** within a **Spring Boot** application. The project implements a **store management system** that leverages **MySQL** as a relational database for structured entities (stores, products, inventory, customers, orders) and **MongoDB** as a document-oriented database for flexible data (product reviews). The application illustrates the use of **Spring Data JPA** with Hibernate for relational persistence, **Spring Data MongoDB** for document persistence, **stored procedures** for analytical reporting, and a **RESTful API** consumed by a static frontend served from the Spring Boot `static` directory. The project serves as a **pedagogical reference** for the integration of multiple database technologies within a single Java enterprise application.

## Overview

The `java-database-final` project is a **self-contained Spring Boot application** that provides a complete store management platform. It enables administrators to:

- Register new stores and manage their inventory.
- Create, update, and delete products in a central catalogue.
- Associate products with stores through inventory entries with stock levels.
- Place customer orders that decrement inventory and generate order records.
- Retrieve customer reviews from MongoDB and enrich them with customer names from MySQL.
- Execute analytical reports powered by MySQL stored procedures.

> ⚠️ **Disclaimer:** This application is **not intended for production deployment**. Its primary purpose is to demonstrate the effective integration of relational and NoSQL databases, stored procedures, and RESTful API design within a Spring Boot environment.

## Technologies and Dependencies

| Technology | Version | Purpose |
|------------|---------|---------|
| **Java** | 17 | Programming language (compiled with SDK 21) |
| **Spring Boot** | 3.4.3 | Application framework and dependency management |
| **Spring Data JPA** | *(managed by Spring Boot)* | Relational persistence with Hibernate |
| **Spring Data MongoDB** | *(managed by Spring Boot)* | Document-oriented persistence |
| **MySQL Connector/J** | *(managed by Spring Boot)* | JDBC driver for MySQL |
| **Spring Boot Actuator** | *(managed by Spring Boot)* | Health checks and monitoring endpoints |
| **Spring Boot Validation** | *(managed by Spring Boot)* | Bean Validation (JSR 380) |
| **Maven** | via wrapper | Build and dependency management |
| **Bootstrap 5** | CDN | Frontend styling and responsive layout |

### Key Dependencies

The `pom.xml` declares the following starters:

- `spring-boot-starter-web`
- `spring-boot-starter-data-jpa`
- `spring-boot-starter-data-mongodb`
- `spring-boot-starter-validation`
- `spring-boot-starter-actuator`
- `mysql-connector-j`
- `spring-boot-devtools`
- `spring-boot-starter-test`

## Project Structure

The source tree follows the conventional **layered architecture** of a Spring Boot application:

```text
back-end/
├── src/
│   ├── main/
│   │   ├── java/com/project/code/
│   │   │   ├── config/               # Database and web configuration
│   │   │   │   ├── DatabaseInfoLogger.java
│   │   │   │   ├── RepositoryConfig.java
│   │   │   │   └── WebConfig.java
│   │   │   ├── Controller/           # REST controllers
│   │   │   │   ├── GlobalExceptionHandler.java
│   │   │   │   ├── InventoryController.java
│   │   │   │   ├── ProductController.java
│   │   │   │   ├── ReportController.java
│   │   │   │   ├── ReviewController.java
│   │   │   │   └── StoreController.java
│   │   │   ├── Model/                # Domain entities and DTOs
│   │   │   │   ├── Customer.java
│   │   │   │   ├── Inventory.java
│   │   │   │   ├── OrderDetails.java
│   │   │   │   ├── OrderItem.java
│   │   │   │   ├── Product.java
│   │   │   │   ├── Review.java
│   │   │   │   ├── Store.java
│   │   │   │   ├── CombinedRequest.java
│   │   │   │   ├── PlaceOrderRequestDTO.java
│   │   │   │   └── PurchaseProductDTO.java
│   │   │   ├── Repo/                 # Repository interfaces
│   │   │   │   ├── jpa/              # JPA repositories
│   │   │   │   │   ├── CustomerRepository.java
│   │   │   │   │   ├── InventoryRepository.java
│   │   │   │   │   ├── OrderDetailsRepository.java
│   │   │   │   │   ├── OrderItemRepository.java
│   │   │   │   │   ├── ProductRepository.java
│   │   │   │   │   ├── ReportRepository.java
│   │   │   │   │   └── StoreRepository.java
│   │   │   │   └── mongo/            # MongoDB repositories
│   │   │   │       └── ReviewRepository.java
│   │   │   ├── Service/              # Business logic
│   │   │   │   ├── OrderService.java
│   │   │   │   └── ServiceClass.java
│   │   │   └── CodeApplication.java
│   │   └── resources/
│   │       ├── static/               # Frontend files (HTML, CSS, JS)
│   │       │   ├── index.html
│   │       │   ├── add-product.html
│   │       │   ├── edit-product.html
│   │       │   ├── add-parent-product.html
│   │       │   ├── edit-parent-product.html
│   │       │   ├── reviews.html
│   │       │   ├── frontend.css
│   │       │   ├── script.js
│   │       │   └── images/
│   │       │       └── Logo.png
│   │       └── application.properties
│   └── test/
└── pom.xml
```

| Package / Folder | Responsibility |
|------------------|----------------|
| `config` | Database logging, repository scanning configuration, CORS |
| `Controller` | HTTP request handling and REST endpoints |
| `Model` | Domain entities (JPA) and MongoDB documents |
| `Repo.jpa` | Spring Data JPA repositories |
| `Repo.mongo` | Spring Data MongoDB repository |
| `Service` | Business logic and transaction management |
| `resources/static` | Frontend application served by Spring Boot |

## Architectural Pattern: Layered Architecture

The application adheres strictly to a **layered architecture**:

- **Model** — Represented by the JPA entities (`Customer`, `Store`, `Product`, `Inventory`, `OrderDetails`, `OrderItem`) and the MongoDB document (`Review`), which encapsulate the data and domain logic.
- **Repository** — The `Repo.jpa` and `Repo.mongo` packages contain Spring Data repository interfaces that provide CRUD operations and custom queries.
- **Service** — The `ServiceClass` centralises validation logic, while `OrderService` implements the transactional order placement workflow, coordinating six repositories.
- **Controller** — The REST controllers (`StoreController`, `ProductController`, `InventoryController`, `ReviewController`, `ReportController`) manage incoming HTTP requests, delegate business operations to the service layer, and return structured JSON responses.
- **View** — The frontend (HTML, CSS, JavaScript) is served from `src/main/resources/static/` and communicates with the backend via `fetch` calls.

## Data Model

### Relational Entities (MySQL)

| Entity | Description | Key Relationships |
|--------|-------------|-------------------|
| **`Store`** | Physical or virtual store | One-to-many with `Inventory` |
| **`Product`** | Commercial product with unique SKU | One-to-many with `Inventory` |
| **`Inventory`** | Stock level of a product at a store | Many-to-one with `Product` and `Store` |
| **`Customer`** | Person who places orders | One-to-many with `OrderDetails` |
| **`OrderDetails`** | Order header with total price and timestamp | Many-to-one with `Customer` and `Store`; one-to-many with `OrderItem` |
| **`OrderItem`** | Line item of an order | Many-to-one with `OrderDetails` and `Product` |

### Document Entity (MongoDB)

| Document | Description | Fields |
|----------|-------------|--------|
| **`Review`** | Customer review of a product | `customerId`, `productId`, `storeId`, `rating`, `comment` |

## ⚙️ Service Layer

The service layer comprises two classes:

- **`ServiceClass`** — Provides validation methods:
  - `validateInventory(Inventory)` — Checks whether an inventory record already exists for a product-store pair.
  - `validateProduct(Product)` — Checks whether a product with the same name already exists.
  - `validateProductId(long)` — Checks whether a product exists by its identifier.
  - `getInventoryId(Inventory)` — Retrieves the inventory record for a product-store pair.

- **`OrderService`** — Implements the transactional `saveOrder(PlaceOrderRequestDTO)` method, which:
  1. Retrieves or creates the customer by email.
  2. Validates the existence of the store.
  3. Persists an `OrderDetails` header with the current timestamp.
  4. Iterates over the purchased products, decrements the inventory stock level, and creates the corresponding `OrderItem` records.

  The method is annotated with `@Transactional`, ensuring that either all operations succeed or the entire transaction is rolled back.

## Controller Endpoints

### `StoreController` — base path `/store`

| HTTP Method | Path | Description |
|:-----------:|------|-------------|
| `POST` | `/store` | Registers a new store. |
| `GET` | `/store/validate/{storeId}` | Validates the existence of a store. |
| `POST` | `/store/placeOrder` | Places a customer order. |

### `ProductController` — base path `/product`

| HTTP Method | Path | Description |
|:-----------:|------|-------------|
| `POST` | `/product` | Creates a new product. |
| `GET` | `/product/product/{id}` | Retrieves a product by its identifier. |
| `PUT` | `/product` | Updates an existing product. |
| `GET` | `/product/category/{name}/{category}` | Filters products by name and category. |
| `GET` | `/product` | Lists all products. |
| `GET` | `/product/filter/{category}/{storeid}` | Filters products by category and store. |
| `DELETE` | `/product/{id}` | Deletes a product and its inventory entries. |
| `GET` | `/product/searchProduct/{name}` | Searches products by name pattern. |

### `InventoryController` — base path `/inventory`

| HTTP Method | Path | Description |
|:-----------:|------|-------------|
| `PUT` | `/inventory` | Updates a product and its inventory entry. |
| `POST` | `/inventory` | Creates a new inventory entry. |
| `GET` | `/inventory/{storeid}` | Retrieves all products available in a store. |
| `GET` | `/inventory/filter/{category}/{name}/{storeid}` | Filters products by category and name within a store. |
| `GET` | `/inventory/search/{name}/{storeId}` | Searches products by name pattern within a store. |
| `DELETE` | `/inventory/{id}` | Removes a product from inventory. |
| `GET` | `/inventory/validate/{quantity}/{storeId}/{productId}` | Validates available stock for a given quantity. |

### `ReviewController` — base path `/reviews`

| HTTP Method | Path | Description |
|:-----------:|------|-------------|
| `GET` | `/reviews/{storeId}/{productId}` | Retrieves reviews for a product at a store, enriched with customer names. |

### `ReportController` — base path `/report`

| HTTP Method | Path | Description |
|:-----------:|------|-------------|
| `GET` | `/report/monthlySalesByStore/{year}/{month}` | Monthly sales for each store. |
| `GET` | `/report/aggregateSalesByCompany/{year}/{month}` | Aggregate sales for the entire company. |
| `GET` | `/report/topProductsByCategory/{month}/{year}` | Top-selling products by category. |
| `GET` | `/report/topProductByStore/{month}/{year}` | Top-selling product by store. |

## Stored Procedures

Four **stored procedures** are defined in MySQL for analytical reporting:

| Procedure | Parameters | Description |
|-----------|-----------|-------------|
| **`GetMonthlySalesForEachStore`** | `year_param`, `month_param` | Returns total sales per store for a given month and year. |
| **`GetAggregateSalesForCompany`** | `year_param`, `month_param` | Returns the total sales for the entire company. |
| **`GetTopSellingProductsByCategory`** | `target_month`, `target_year` | Returns the top-selling product(s) per category. |
| **`GetTopSellingProductByStore`** | `target_month`, `target_year` | Returns the top-selling product(s) per store. |

These procedures are invoked through `ReportRepository` using the `@Procedure` annotation and are exposed via the `/report` endpoints.

## Database Host Configuration: Local vs. Remote

One of the design goals of this project is to allow the application to connect either to a **local** database instance (running on the same machine as the developer) or to a **remote** database instance (running on a dedicated server within the local network), **without requiring any change to the source code**.

### The `db.host` property

The connection host is controlled by a single property defined in `src/main/resources/application.properties`:

```properties
# Database host (single line to modify)
db.host=${DB_HOST:192.168.0.10}
```

This property is **referenced by both the MySQL JDBC URL and the MongoDB connection URI**, so switching between a local and a remote server requires modifying only this one line (or, preferably, overriding the `DB_HOST` environment variable without touching the file).

### Switching between remote and local

There are four ways to select the target host, in increasing order of precedence:

#### Option 1 – Edit `application.properties`

Change the default value:

```properties
# Remote server (default)
db.host=${DB_HOST:192.168.0.10}

# Local server (alternative)
# db.host=${DB_HOST:localhost}
```

#### Option 2 – Environment variable

Override the host without touching the file:

```bash
# Git Bash / Linux / macOS
export DB_HOST=localhost
./mvnw spring-boot:run

# PowerShell (Windows)
$env:DB_HOST="localhost"
.\mvnw spring-boot:run

# CMD (Windows)
set DB_HOST=localhost
mvnw spring-boot:run
```

#### Option 3 – Command-line argument

```bash
java -jar target/code-0.0.1-SNAPSHOT.jar --db.host=localhost
```

#### Option 4 – Spring profile

Spring Boot profiles provide the cleanest way to manage environment-specific configurations. Create two files:

**`application-local.properties`**

```properties
db.host=localhost
spring.datasource.password=local_password
spring.data.mongodb.uri=mongodb://root:local_mongo_password@localhost:27017/store_reviews?authSource=admin
```

**`application-remote.properties`**

```properties
db.host=192.168.0.10
spring.datasource.password=remote_password
spring.data.mongodb.uri=mongodb://root:remote_mongo_password@192.168.0.10:27017/store_reviews?authSource=admin
```

Activate the desired profile:

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=local
```

or

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=remote
```

### Verifying the active host at startup

The application includes a `DatabaseInfoLogger` class that prints the active database host to the console as soon as the Spring context is fully initialized:

```text
================================================
  Database connection configuration
     Active host: 192.168.0.10
     MySQL:       192.168.0.10:3306/store_management
     MongoDB:     192.168.0.10:27017/store_reviews
================================================
```

This makes it trivial to confirm which environment the application is connected to, avoiding configuration mistakes.

## Building and Running the Application

The project includes a **Maven wrapper** (`mvnw`), so Maven need not be installed globally.

### Prerequisites

- Java 21 (or Java 17)
- Docker (or a local installation of MySQL and MongoDB)
- MySQL 8.x and MongoDB 7.x running (e.g., in Docker containers)

### Database Configuration

The file `src/main/resources/application.properties` contains the connection settings. Replace the placeholder credentials with the actual passwords:

```properties
# Database host (single line to modify)
db.host=${DB_HOST:192.168.0.10}

# MySQL
spring.datasource.url=jdbc:mysql://${db.host}:3306/store_management?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
spring.datasource.username=root
spring.datasource.password={MYSQL_PASSWORD}

# MongoDB
spring.data.mongodb.uri=mongodb://root:{MONGODB_PASSWORD}@${db.host}:27017/store_reviews?authSource=admin
```

### Build

```bash
./mvnw clean package
```

### Run with the Maven plugin

```bash
./mvnw spring-boot:run
```

### Run the generated JAR

```bash
java -jar target/code-0.0.1-SNAPSHOT.jar
```

Once the application is running, it is accessible at:

```text
http://localhost:8080
```

### Health Check

The Spring Boot Actuator endpoint provides the status of both databases:

```text
http://localhost:8080/actuator/health
```

Expected response:

```json
{
  "status": "UP",
  "components": {
    "db": { "status": "UP" },
    "mongo": { "status": "UP" }
  }
}
```

## Usage

The frontend is served from the backend and provides **four main tabs**:

### Store

- Register a new store with name and address.
- The store is assigned an auto-generated identifier.

### Products

- List all products in the catalogue.
- Filter products by category or search by name.
- Add, edit, or delete products.
- Product SKU must be unique.

### Inventory

- Enter a store ID to view its inventory.
- Filter by category or search by product name.
- Add products to a store's inventory with an initial stock level.
- Update stock levels and product details.
- Validate available quantity before placing an order.

### Place Order

- Validate the store ID.
- Enter customer details (name, email, phone).
- Add one or more products with quantities.
- The system decrements inventory and persists the order.

### Reviews

- Accessible from the Inventory tab by clicking the **Reviews** button for a product.
- Displays customer reviews retrieved from MongoDB, enriched with customer names from MySQL.

## Frontend Interface

The frontend is a **static single-page application** served from:

```text
src/main/resources/static/
```

Key files:

- `index.html` — Main page with tabs for Store, Products, Inventory, and Place Order.
- `reviews.html` — Review display page.
- `script.js` — Core JavaScript logic for API communication.
- `frontend.css` — Global stylesheet.

All API calls are made to **relative URLs** (e.g., `/store`, `/product`, `/inventory`), leveraging the fact that the frontend and backend share the same origin (`http://localhost:8080`). Since frontend and backend are served from the same host, **no CORS configuration is required** for the frontend to communicate with the API.

## Testing

The `src/test` directory contains the default Spring Boot test class generated by Spring Initializr. Additional unit and integration tests are **not included** in this demonstration. Developers extending the project are encouraged to add tests using:

- **JUnit 5**
- **Mockito**
- **Spring Boot Test** (`@SpringBootTest`, `@DataJpaTest`, `@WebMvcTest`)

## Limitations and Future Work

The current implementation has several limitations inherent to its demonstrative nature:

- [ ] **Stock level validation**: The `OrderService` does not prevent stock levels from becoming negative. A production system should validate stock availability before confirming an order.
- [ ] **`SUM(DISTINCT)` in stored procedures**: The monthly sales procedures use `SUM(DISTINCT total_price)`, which collapses orders with identical totals. A more accurate calculation would use `SUM(total_price)`.
- [ ] **Credentials in plain text**: Database passwords are stored in `application.properties`. A production deployment should use environment variables or a secrets manager.
- [ ] **Error handling**: The global exception handler captures only `HttpMessageNotReadableException`. A production application should implement comprehensive exception handling for all failure scenarios.
- [ ] **No authentication or authorization**: The application does not implement user authentication. A production system would require Spring Security or a similar mechanism.

## License

This project is distributed under the terms of the license included in the [`LICENSE`](LICENSE) file at the root of the repository.

## ✅ Conclusion

The `java-database-final` repository provides a comprehensive example of integrating multiple database technologies within a Spring Boot application. By combining relational persistence (MySQL) with document-oriented persistence (MongoDB), leveraging stored procedures for analytics, and exposing a RESTful API consumed by a static frontend, the project demonstrates a realistic enterprise architecture and offers a solid foundation for further exploration and extension.

The inclusion of a configurable database host property (`db.host`) further demonstrates a practical approach to environment management: the same codebase can seamlessly connect either to a local development instance or to a remote production-like server without any modification, thereby illustrating a principle of **configuration over code**.

**⭐ If you found this project useful, consider giving it a star! ⭐**

Made with ❤️ using Spring Boot