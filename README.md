# 🗄️ Spring Boot Data JPA

A Spring Boot application demonstrating Spring Data JPA concepts including derived queries, JPQL, pagination, sorting, database indexing, and audit timestamps with MySQL.

---

## ☁️ Core Concepts Overview

| Concept | Description |
|---------|-------------|
| Derived Queries | Auto-generated queries from method names |
| JPQL | Custom queries using `@Query` annotation |
| Pagination | `Pageable` + `PageRequest` for page-based results |
| Sorting | `Sort.by()` for single and multi-field sorting |
| Table Constraints | `@UniqueConstraint` and `@Index` on entity |
| Audit Timestamps | `@CreationTimestamp` and `@UpdateTimestamp` |

---

## 🛠️ What I Built

### 1. Product Entity
- `@Table` with `uniqueConstraints` and `indexes`
- `@CreationTimestamp` and `@UpdateTimestamp` for auto audit fields
- `@Column` with `nullable`, `length`, and custom `name`
- `@Builder` pattern with Lombok

### 2. Product Repository
- Derived queries — method name based query generation
- JPQL — custom `@Query` with named parameters
- Pagination — `Pageable` parameter support
- Sorting — `Sort` parameter support

### 3. Product Controller
- GET `/products` with optional filtering, sorting, and pagination
- `@RequestParam` — `title`, `sortBy`, `pageNumber`

---

## 🔧 Core Concepts Demonstrated

### Derived Queries
```java
// Filter by title with pagination
List<ProductEntity> findByTitle(String title, Pageable pageable);

// Order by title after a date
List<ProductEntity> findByCreatedAtAfterOrderByTitle(LocalDateTime after);

// OR condition
List<ProductEntity> findByQuantityGreaterThanOrPriceLessThan(int quantity, BigDecimal price);

// Case-insensitive search with pagination
List<ProductEntity> findByTitleContainingIgnoreCase(String title, Pageable pageable);
```

### JPQL with @Query
```java
@Query("select e.title, e.price from ProductEntity e where e.title=:title and e.price=:price")
Optional<ProductEntity> findByTitleAndPrice(String title, BigDecimal price);
```

### Pagination & Sorting
```java
return productRepository.findByTitleContainingIgnoreCase(
    title,
    PageRequest.of(pageNumber, PAGE_SIZE, Sort.by(sortBy))
);
```

### Table Constraints & Indexes
```java
@Table(
    name = "product_table",
    uniqueConstraints = {
        @UniqueConstraint(name = "title_price_unique", columnNames = {"title_x", "price"})
    },
    indexes = {
        @Index(name = "sku_index", columnList = "sku")
    }
)
```

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/products` | Get all products with optional filter, sort, and pagination |

**Query Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `title` | `""` | Filter by title (case-insensitive) |
| `sortBy` | `id` | Sort field |
| `pageNumber` | `0` | Page number |

---

## 🧪 Tests

```java
// Save entity test
void testRepository() — saves a ProductEntity and prints result

// Custom query test  
void getRepository() — tests findByTitleContainingIgnoreCase

// JPQL test
void getSingleFromRepository() — tests findByTitleAndPrice JPQL query
```

---

## 🚀 Quick Start

**Prerequisites:** Java 21+, Maven, MySQL

```bash
git clone https://github.com/MansiArora-dev/springboot-data-jpa.git
cd springboot-data-jpa
```

**Setup MySQL:**
1. Create database: `CREATE DATABASE <your_database_name>;`
2. Create `application-local.properties` in `src/main/resources/` with your credentials:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/<your_database_name>?useSSL=false
spring.datasource.username=<your_username>
spring.datasource.password=<your_password>
```
> ⚠️ Add `application-local.properties` to `.gitignore` to avoid committing credentials

**Using IntelliJ IDEA (Recommended):**
1. Open project in IntelliJ
2. Run → **Edit Configurations**
3. Environment variables: `SPRING_PROFILES_ACTIVE=local`
4. Click **Run ▶️**

**Using Maven:**
```bash
mvn spring-boot:run -Dspring-boot.run.profiles=local
```

---

## 📂 Project Structure

```
src/main/java/com/springboot/jpa/
├── controllers/
│   └── ProductController.java     # REST endpoint with pagination
├── entities/
│   └── ProductEntity.java         # JPA Entity with constraints
├── repositories/
│   └── ProductRepository.java     # Derived queries and JPQL
└── SpringBootJpaApplication.java  # Main entry point
src/main/resources/
├── application.properties         # App configuration
└── data.sql                       # Seed data 
src/test/
└── SpringBootJpaApplicationTests.java  # Repository tests
```

## 💻 Technologies

- **Java 21** | **Spring Boot** | **Maven**
- **Spring Data JPA** | **MySQL** | **Hibernate**

---

## 🌟 Key Takeaways
- **Derived Queries** — Spring generates SQL from method names automatically
- **JPQL** — Custom queries when derived queries are not enough
- **Pagination** — Efficient data retrieval with `PageRequest`
- **Audit Timestamps** — Auto-managed `createdAt` and `updatedAt` fields

---

## 👩‍💻 Developer
**Mansi Arora** — Software Engineer