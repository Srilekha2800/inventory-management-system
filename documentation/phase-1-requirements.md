# Inventory Management System

## Phase 1 — Requirements & Project Documentation

---

## 1. Project Overview

The **Inventory Management System** is a backend application designed to help small and medium-sized businesses manage products, categories, inventory levels, users, and stock transactions.

The initial version will be developed as a **console-based Java application** with **MySQL** as the persistent database.

The project is being developed progressively to establish a practical understanding of Java backend development and backend architecture. The initial implementation will use Core Java, MySQL, and JDBC. A future version may be migrated to **Spring Boot** and exposed through REST APIs for frontend integration.

### Initial Technology Stack

* Java 21
* Core Java
* MySQL
* JDBC

### Planned Technologies

* Spring Boot
* Spring Data JPA / Hibernate
* REST APIs
* Frontend integration

---

## 2. Problem Statement

Businesses require reliable mechanisms to manage product information, inventory levels, stock movements, and user activities.

Manual inventory management through spreadsheets or separate records can make it difficult to maintain accurate stock information, track inventory movements, identify low-stock products, and maintain data consistency.

The Inventory Management System provides a centralized solution for managing products, categories, inventory operations, users, and stock transaction history.

---

## 3. Project Objectives

The primary objectives of the system are to:

1. Manage products and categories.
2. Maintain current inventory quantities.
3. Support authorized stock-in and stock-out operations.
4. Prevent invalid inventory operations.
5. Maintain stock transaction history.
6. Identify products requiring stock replenishment.
7. Implement user authentication and role-based authorization.
8. Persist application data using MySQL.
9. Separate presentation, business logic, and database operations.
10. Provide basic inventory reports.

The project will also demonstrate practical implementation of object-oriented programming, database integration, layered architecture, exception handling, validation, transactions, and backend development practices.

---

# 4. Project Scope

## 4.1 Product Management

The system will support:

* Adding products.
* Viewing products.
* Searching products.
* Updating product information.
* Deactivating products.

## 4.2 Category Management

The system will support:

* Adding categories.
* Viewing categories.
* Updating categories.
* Deactivating categories where required.

## 4.3 Inventory Management

The system will support:

* Viewing current stock.
* Performing stock-in operations.
* Performing stock-out operations.
* Viewing stock transaction history.
* Identifying low-stock products.

## 4.4 User Management

The system will support:

* User authentication.
* Role management.
* Creating users.
* Viewing users.
* Updating users.
* Deactivating users.

## 4.5 Reporting

The system will provide basic inventory information, including:

* Total number of products.
* Total number of categories.
* Low-stock products.
* Total inventory value.
* Stock movement information.

## 4.6 Technical Scope

The project will demonstrate:

* Object-oriented programming.
* Java collections.
* Interfaces and abstraction.
* Exception handling.
* Input validation.
* MySQL database integration.
* JDBC.
* DAO pattern.
* Service layer.
* Database transactions.
* Authentication and authorization.
* Logging.
* Testing.
* Layered architecture.

---

# 5. Out of Scope

The following features are outside the scope of the initial version:

* Payment processing.
* Shopping cart functionality.
* Customer management.
* Supplier management.
* Purchase order management.
* Invoice generation.
* Delivery tracking.
* Barcode scanning.
* Email notifications.
* Cloud deployment.
* Microservices architecture.
* Advanced analytics.

These features may be considered as future enhancements.

---

# 6. Users and Roles

The system will initially support two roles.

## 6.1 Admin

The Admin will have administrative privileges and will be responsible for:

* Managing products.
* Managing categories.
* Managing inventory.
* Managing users.
* Viewing reports.

## 6.2 Employee

The Employee will perform day-to-day inventory operations and will be able to:

* View products.
* Search products.
* Check stock.
* Perform stock-in operations.
* Perform stock-out operations.
* View low-stock products.
* View stock history.

Employees will not have access to administrative operations such as user management.

---

# 7. Authentication and Authorization

## 7.1 Authentication

The system will authenticate users using their username and password.

The authentication process will:

1. Validate the login input.
2. Locate the user account.
3. Verify the credentials.
4. Retrieve the user's role.
5. Grant access when authentication succeeds.
6. Reject access when authentication fails.

## 7.2 Authorization

Authorization will determine which operations a user can perform based on their assigned role.

Role-based access control will be applied to administrative and inventory operations.

---

# 8. Functional Requirements

## FR-01 — User Login

The system shall allow registered users to authenticate using a username and password.

## FR-02 — Add Product

The Admin shall be able to create a product using:

* Product name.
* Price.
* Category.
* Initial stock.
* Reorder level.

The system shall validate the provided information and verify that the selected category exists before creating the product.

## FR-03 — View Products

Authorized users shall be able to view active products and their relevant information, including current stock.

## FR-04 — Search Product

Authorized users shall be able to search for products using supported search criteria such as:

* Product ID.
* Product name.

## FR-05 — Update Product

The Admin shall be able to update product information such as:

* Product name.
* Price.
* Category.
* Reorder level.

Current stock shall not normally be modified through general product updates. Inventory quantity shall be changed through stock-in and stock-out operations.

## FR-06 — Deactivate Product

The Admin shall be able to deactivate products.

The implementation may use logical deactivation rather than permanent deletion where required to preserve historical transaction records.

## FR-07 — Add Category

The Admin shall be able to create product categories containing:

* Category name.
* Description.

Category names shall be unique.

## FR-08 — View Categories

Authorized users shall be able to view available categories.

## FR-09 — Update Category

The Admin shall be able to update category information, including the category name and description.

## FR-10 — Stock In

Authorized users shall be able to increase the stock quantity of an existing product.

A successful stock-in operation shall:

1. Validate the product.
2. Validate the quantity.
3. Update the current stock.
4. Create a stock transaction record.
5. Persist the changes.

## FR-11 — Stock Out

Authorized users shall be able to decrease the stock quantity of an existing product.

The system shall ensure that the requested quantity does not exceed the available stock.

A successful stock-out operation shall:

1. Validate the product.
2. Validate the requested quantity.
3. Verify sufficient stock.
4. Update the current stock.
5. Create a stock transaction record.
6. Persist the changes.

## FR-12 — View Current Stock

Authorized users shall be able to view the current stock quantity of products.

## FR-13 — Low Stock Detection

The system shall identify products whose current stock is less than or equal to their configured reorder level.

**Low-stock condition:**

`Current Stock <= Reorder Level`

## FR-14 — View Stock History

Authorized users shall be able to view historical inventory transactions.

Each transaction shall contain relevant information such as:

* Product.
* Transaction type.
* Quantity.
* User.
* Date and time.

## FR-15 — User Management

The Admin shall be able to:

* Create users.
* View users.
* Update users.
* Deactivate users.

User information shall include:

* Username.
* Password credentials.
* Role.
* Account status.

## FR-16 — Reports

The system shall provide basic inventory reports, including:

* Total number of products.
* Total number of categories.
* Low-stock products.
* Total inventory value.
* Stock movement information.

Inventory value shall be calculated based on product price and current stock quantity.

---

# 9. Non-Functional Requirements

## NFR-01 — Reliability

The system shall prevent invalid operations and maintain accurate inventory information.

## NFR-02 — Data Consistency

Inventory updates and their corresponding transaction records shall remain consistent.

Stock changes and transaction creation shall be treated as a single logical database operation and implemented using database transactions where required.

## NFR-03 — Security

The system shall restrict operations according to user roles and protect authenticated user access.

## NFR-04 — Maintainability

The application shall separate:

* User interaction.
* Business logic.
* Database operations.

The planned architecture is:

```text
Console / UI
     ↓
Service Layer
     ↓
DAO Layer
     ↓
JDBC
     ↓
MySQL
```

## NFR-05 — Usability

The console interface shall provide clear menus, input prompts, and operation results.

## NFR-06 — Performance

The application shall use appropriate database queries and avoid unnecessary database operations.

Enterprise-level performance optimization is outside the scope of the initial version.

---

# 10. System Modules

The system will contain the following major modules:

1. Authentication
2. Product Management
3. Category Management
4. Inventory Management
5. User Management
6. Reporting

Modules will be implemented incrementally throughout the development phases.

---

# 11. Initial User Flow

The application will require authentication before accessing protected operations.

After successful authentication, the system will provide functionality based on the user's role.

### Admin Capabilities

* Product Management
* Category Management
* Inventory Management
* User Management
* Reports
* Logout

### Employee Capabilities

* View Products
* Search Products
* Check Stock
* Stock In
* Stock Out
* View Low-Stock Products
* View Stock History
* Logout

---

# 12. Inventory Operation Workflow

Inventory operations will follow a controlled backend workflow.

### Stock-In

```text
User Request
     ↓
Validate Input
     ↓
Verify Product
     ↓
Update Current Stock
     ↓
Create Stock Transaction
     ↓
Commit Transaction
     ↓
Return Result
```

### Stock-Out

```text
User Request
     ↓
Validate Input
     ↓
Verify Product
     ↓
Check Available Stock
     ↓
Update Current Stock
     ↓
Create Stock Transaction
     ↓
Commit Transaction
     ↓
Return Result
```

If an inventory operation fails, the system shall prevent an inconsistent stock state.

---

# 13. Initial Backend Architecture

The application will follow a layered architecture.

```text
Console / UI
     ↓
Service Layer
     ↓
DAO Layer
     ↓
JDBC
     ↓
MySQL
```

### Console / UI Layer

Responsible for:

* User interaction.
* Input collection.
* Menu navigation.
* Displaying results.

### Service Layer

Responsible for:

* Business rules.
* Validation.
* Business operations.
* Coordination between application components.

### DAO Layer

Responsible for:

* Database operations.
* SQL execution.
* Data retrieval and persistence.

### JDBC

Provides the communication mechanism between the Java application and MySQL.

### MySQL

Provides persistent storage for application data.

The architecture will be implemented progressively as the project develops.

---

# 14. Development Approach

The project will be developed incrementally through the following stages:

```text
Requirements
     ↓
Domain Model Design
     ↓
Database Design
     ↓
Core Java Implementation
     ↓
JDBC Integration
     ↓
DAO Layer
     ↓
Service Layer
     ↓
Database Transactions
     ↓
Authentication
     ↓
Authorization
     ↓
Inventory Operations
     ↓
Reports
     ↓
Testing
     ↓
Refactoring
     ↓
Spring Boot REST API
```

Each stage will build upon the previous stage while keeping the implementation manageable and maintainable.

---

# 15. Use Cases

## UC-01 — User Login

**Actors:** Admin, Employee

**Objective:** Authenticate a registered user and provide access according to their role.

**Main Flow:**

1. User submits credentials.
2. System validates the credentials.
3. System identifies the user's role.
4. System grants access to the appropriate functionality.

---

## UC-02 — Add Product

**Actor:** Admin

**Objective:** Add a new product to the inventory.

The system validates the product information and category before creating the product.

---

## UC-03 — View and Search Products

**Actors:** Admin, Employee

**Objective:** View available products and retrieve products using supported search criteria.

---

## UC-04 — Update Product

**Actor:** Admin

**Objective:** Modify existing product information while maintaining inventory control.

---

## UC-05 — Manage Categories

**Actor:** Admin

**Objective:** Create, view, and update product categories.

---

## UC-06 — Stock In

**Actors:** Admin, Employee

**Objective:** Increase product inventory when new stock is received.

The operation shall update current stock and create a corresponding stock transaction.

---

## UC-07 — Stock Out

**Actors:** Admin, Employee

**Objective:** Decrease product inventory when stock leaves the inventory.

The system shall verify sufficient stock before completing the operation.

---

## UC-08 — View Current Stock

**Actors:** Admin, Employee

**Objective:** View the current inventory quantity of products.

---

## UC-09 — Low Stock Detection

**Actors:** Admin, Employee

**Objective:** Identify products whose stock has reached or fallen below their reorder level.

---

## UC-10 — View Stock History

**Actors:** Admin, Employee

**Objective:** Retrieve historical stock transactions for inventory tracking and auditing.

---

## UC-11 — User Management

**Actor:** Admin

**Objective:** Create, view, update, and deactivate system users.

---

## UC-12 — Reports

**Actor:** Admin

**Objective:** Retrieve basic inventory information for monitoring and management.

---

# 16. Business Rules

## BR-01 — Product Rules

* Product name must not be empty.
* Product price must be greater than or equal to zero.
* Product stock must be greater than or equal to zero.
* Reorder level must be greater than or equal to zero.
* Every product must reference an existing category.
* Product information must be validated before persistence.

## BR-02 — Category Rules

* Category name must not be empty.
* Category names must be unique.
* Products may reference only existing categories.

## BR-03 — Stock-In Rules

* Stock-in quantity must be greater than zero.
* The product must exist.
* Current stock shall increase by the stock-in quantity.
* A successful stock-in operation shall create a transaction record.

## BR-04 — Stock-Out Rules

* Stock-out quantity must be greater than zero.
* The product must exist.
* Requested quantity must not exceed available stock.
* Current stock shall decrease by the stock-out quantity.
* A successful stock-out operation shall create a transaction record.
* Failed stock-out operations shall not modify current stock.

## BR-05 — Low-Stock Rule

A product is considered low stock when:

`Current Stock <= Reorder Level`

## BR-06 — User Rules

* Username must not be empty.
* Username must be unique.
* Password credentials must be provided.
* Every user must have a valid role.
* Supported roles are `ADMIN` and `EMPLOYEE`.

## BR-07 — Authorization Rules

Administrative operations shall be restricted to Admin users.

Employee users shall have access only to operations permitted for the Employee role.

## BR-08 — Product Deactivation

Deactivated products shall not be treated as active products for normal inventory operations.

Deactivation may be preferred over permanent deletion where historical transaction records need to be preserved.

## BR-09 — Inventory Consistency

Current stock and stock transaction records shall remain consistent.

An inventory operation shall update the current stock and create its corresponding transaction record as part of the same logical operation.

Database transactions shall be used to maintain consistency when multiple database operations must succeed together.

## BR-10 — Inventory State and History

The system shall maintain a distinction between:

**Product:** represents the current inventory state.

**StockTransaction:** represents historical inventory changes.

This separation allows the system to maintain both current inventory information and its historical movement.

---

# 17. Phase 1 Conclusion

Phase 1 establishes the functional and business requirements for the Inventory Management System.

The requirements define:

* Project objectives and scope.
* Users and roles.
* Authentication and authorization requirements.
* Functional requirements.
* Non-functional requirements.
* System modules.
* User flows.
* Use cases.
* Business rules.
* Inventory operation requirements.
* Initial backend architecture.
* Development approach.


