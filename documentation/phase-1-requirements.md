# Inventory Management System

## Phase 1 — Requirements & Project Documentation

---

## 1. Project Overview

The **Inventory Management System** is a backend application designed to help a small or medium-sized business manage its products, categories, stock quantities, users, and inventory transactions.

The first version of the application will be developed as a **console-based Java application**. MySQL will be used as the database for permanently storing application data.

The project is being developed progressively to understand how a real-world Java backend is designed and implemented. The initial version will focus on Core Java, MySQL, and JDBC. Later, the backend can be converted into a **Spring Boot REST API** and connected to a frontend application.

### Initial Technology Stack

* Java 21
* Core Java
* MySQL
* JDBC

### Planned Future Technologies

* Spring Boot
* Spring Data JPA / Hibernate
* REST APIs
* Frontend integration

---

## 2. Problem Statement

Businesses need to keep track of the products they have, the quantity currently available, and changes made to their inventory.

Managing this information manually using notebooks, spreadsheets, or separate records can make it difficult to:

* Know the current stock of a product.
* Add newly received stock.
* Record products that have been removed from inventory.
* Track previous stock movements.
* Identify products that are running low.
* Know which user performed an inventory operation.
* Maintain consistent and reliable inventory data.

The Inventory Management System provides a centralized application for managing this information.

The system allows authorized users to manage products and categories, perform stock-in and stock-out operations, check current inventory, view stock history, identify low-stock products, and generate basic inventory reports.

---

## 3. Project Objective

The main objective of this project is to build a reliable inventory management backend while learning how a real-world Java backend application is designed.

The system should:

1. Manage products and categories.
2. Maintain the current stock quantity of each product.
3. Allow authorized users to add and remove stock.
4. Prevent invalid inventory operations.
5. Maintain a history of stock movements.
6. Identify products whose stock has reached or fallen below their reorder level.
7. Support user authentication and role-based authorization.
8. Store application data permanently in MySQL.
9. Separate user interaction, business logic, and database operations.
10. Provide basic inventory reports.

A major learning objective of this project is to understand the complete backend workflow:

```text
Real-World Problem
        ↓
Requirements
        ↓
Domain Model
        ↓
Database
        ↓
Java Code
        ↓
Business Logic
        ↓
JDBC
        ↓
DAO
        ↓
Service Layer
        ↓
Transactions
        ↓
Authentication & Authorization
        ↓
Testing
        ↓
Spring Boot REST API
```

---

## 4. Project Scope

The first version of the project will focus on the following areas.

### 4.1 Product Management

The system will support:

* Adding products.
* Viewing products.
* Searching for products.
* Updating product information.
* Deactivating or removing products.

### 4.2 Category Management

The system will support:

* Adding categories.
* Viewing categories.
* Updating categories.
* Deactivating or removing categories.

### 4.3 Inventory Management

The system will support:

* Viewing current stock.
* Adding stock through stock-in operations.
* Removing stock through stock-out operations.
* Viewing stock transaction history.
* Identifying low-stock products.

### 4.4 User Management

The system will support:

* User login.
* User roles.
* Creating users.
* Viewing users.
* Updating users.
* Deactivating users.

### 4.5 Reports

The system will provide basic inventory information such as:

* Total number of products.
* Total number of categories.
* Number of low-stock products.
* Total inventory value.
* Basic stock movement information.

### 4.6 Technical Scope

During development, the project will also demonstrate:

* Object-oriented programming.
* Collections.
* Interfaces.
* Exception handling.
* Input validation.
* MySQL database integration.
* JDBC.
* DAO pattern.
* Service layer.
* Database transactions.
* Authentication.
* Authorization.
* Logging.
* Testing.
* Layered backend architecture.

---

## 5. Out of Scope

To keep the project focused and manageable, the following features are not part of the initial version:

* Online payment processing.
* Shopping cart.
* Customer management.
* Supplier management.
* Purchase order management.
* Sales invoice generation.
* Delivery tracking.
* Barcode scanning.
* Email notification system.
* Cloud deployment.
* Microservices architecture.
* Advanced analytics.

These features may be considered as future enhancements but are not required for the initial project.

---

## 6. Users and Roles

The system will initially have two types of users:

```text
                    Inventory System
                          |
             +------------+------------+
             |                         |
           ADMIN                    EMPLOYEE
```

### 6.1 Admin

The Admin has higher privileges and is responsible for managing the system.

Admin can:

* Manage products.
* Manage categories.
* Manage inventory.
* Manage users.
* View reports.

### 6.2 Employee

The Employee performs normal inventory-related operations.

Employee can:

* View products.
* Search products.
* Check stock.
* Perform stock-in operations.
* Perform stock-out operations.
* View low-stock products.
* View stock history.

Employees cannot perform administrative operations such as managing system users.

---

## 7. Authentication and Authorization

The system will implement both authentication and authorization.

### 7.1 Authentication

Authentication determines:

> **Who is the user?**

A user provides:

Username
Password


The system verifies the credentials against the stored user information.

Example:

Username: employee01
Password: ********
        ↓
System verifies credentials
        ↓
Login successful
        ↓
User role identified


### 7.2 Authorization

Authorization determines:

> **What is the user allowed to do?**

For example:

EMPLOYEE
    ↓
Stock Out       → Allowed
View Products   → Allowed
Manage Users    → Not Allowed


The system will use the user's role to control access to different operations.

---

# 8. Functional Requirements

Functional requirements describe what the system must do.

---

## FR-01 — User Login

The system shall allow registered users to log in using a username and password.

### Input

* Username
* Password

### System Behavior

1. Validate the input.
2. Search for the username.
3. Verify the user's credentials.
4. Identify the user's role.
5. Allow access if the credentials are valid.
6. Reject the login if the credentials are invalid.

---

## FR-02 — Add Product

The Admin shall be able to add a new product.

### Input

* Product name
* Price
* Category
* Initial stock
* Reorder level

### System Behavior

1. Validate the input.
2. Verify that the selected category exists.
3. Create the product.
4. Store the product information.
5. Display a success message.

---

## FR-03 — View Products

Authorized users shall be able to view available products.

Example:

```text
ID    Name          Price      Stock    Category
--------------------------------------------------
1     Keyboard      1200       25       Accessories
2     Mouse         600        12       Accessories
3     Monitor       15000      4        Electronics
```

The product information will eventually be retrieved from MySQL.

---

## FR-04 — Search Product

Authorized users shall be able to search for products.

The initial search options will include:

* Product ID.
* Product name.

Example:

```text
Enter product name:
Keyboard
```

The system will display the matching product or products.

---

## FR-05 — Update Product

The Admin shall be able to update product information.

The update may include:

* Product name.
* Price.
* Category.
* Reorder level.

The current stock quantity should normally **not be directly edited through product update**.

Stock quantity should be changed through:

* Stock In.
* Stock Out.

This ensures that inventory changes can be recorded in the stock transaction history.

---

## FR-06 — Deactivate or Remove Product

The Admin shall be able to remove a product from active inventory.

The final implementation will determine whether products are:

* Permanently deleted, or
* Marked as inactive.

This decision will be finalized during database design because historical stock transactions may refer to existing products.

---

## FR-07 — Add Category

The Admin shall be able to create a new product category.

### Input

* Category name.
* Description.

Example categories:

```text
Electronics
Accessories
Stationery
Furniture
Networking
```

Category names should be unique.

---

## FR-08 — View Categories

Authorized users shall be able to view available categories.

Example:

```text
1. Electronics
2. Accessories
3. Stationery
4. Furniture
5. Networking
```

---

## FR-09 — Update Category

The Admin shall be able to update category information.

The update may include:

* Category name.
* Description.

---

## FR-10 — Stock In

Authorized users shall be able to add stock to an existing product.

Example:

```text
Current Stock = 50
Stock In      = 20

New Stock     = 70
```

The system must also record the stock movement.

Example transaction:

Product: Keyboard
Type: STOCK_IN
Quantity: 20
User: employee01
Date/Time: ...


---

## FR-11 — Stock Out

Authorized users shall be able to remove stock from an existing product.

Example:

Current Stock = 70
Stock Out     = 5

New Stock     = 65


The system must record the stock movement.

### Important Validation

The requested stock-out quantity must not be greater than the available stock.

Example:

Current Stock = 10
Stock Out     = 15


The system must reject the operation.

Stock must never become negative.

---

## FR-12 — View Current Stock

Authorized users shall be able to check the current stock of products.

Example:

Keyboard  → 65
Mouse     → 20
Monitor   → 4


The current stock represents the present state of the inventory.

---

## FR-13 — Low Stock Detection

The system shall identify products whose current stock has reached or fallen below their reorder level.

The rule is:

Current Stock <= Reorder Level


Example:

Current Stock  = 4
Reorder Level  = 5


Result:

LOW STOCK

---

## FR-14 — View Stock History

Authorized users shall be able to view previous inventory movements.

Example:

Date        Product     Type        Quantity    User
------------------------------------------------------
Oct 07      Keyboard    STOCK_IN       20       admin
Oct 08      Keyboard    STOCK_OUT       5       employee01
Oct 09      Mouse       STOCK_OUT       3       employee02


Stock history allows the system to answer:

> What happened to the inventory?

rather than only:

> What is the current inventory?

---

## FR-15 — User Management

The Admin shall be able to manage system users.

The initial operations will include:

* Add user.
* View users.
* Update user.
* Deactivate user.

User information will include:

* Username.
* Password.
* Role.
* Account status.

---

## FR-16 — Reports

The system shall provide basic inventory reports.

The initial reports will include:

* Total number of products.
* Total number of categories.
* Number of low-stock products.
* Total inventory value.
* Basic stock movement information.

The inventory value can be calculated using:


Inventory Value = Sum of (Product Price × Current Stock)


---

# 9. Non-Functional Requirements

Non-functional requirements describe how the system should behave.

### NFR-01 — Reliability

The system should prevent invalid operations and maintain correct inventory information.

For example:

Current Stock = 5
Stock Out = 10


The operation must fail without making the stock negative.

### NFR-02 — Data Consistency

Inventory changes and their corresponding transaction records should remain consistent.

For example, during a stock-out operation:

Update Product Stock
        +
Create Stock Transaction


Both operations should succeed together.

If an operation fails, the database should not be left in an inconsistent state.

### NFR-03 — Security

Users should only be able to perform operations permitted by their roles.

### NFR-04 — Maintainability

The application should separate:

* User interaction.
* Business logic.
* Database operations.

The planned architecture is:

Console/UI
    ↓
Service Layer
    ↓
DAO Layer
    ↓
JDBC
    ↓
MySQL


### NFR-05 — Usability

The console menus and messages should be simple and understandable for users.

### NFR-06 — Performance

The application should use appropriate database queries and avoid unnecessary database operations.

The initial project does not require enterprise-level performance optimization, but standard backend practices should be followed.

---

# 10. Main System Modules

The application will contain the following major modules:

Inventory Management System
│
├── Authentication
│
├── Product Management
│
├── Category Management
│
├── Inventory Management
│
├── User Management
│
└── Reports


These modules will be implemented progressively rather than all at once.

---

# 11. Initial User Flow

When the application starts, the user will see:

================================
    INVENTORY MANAGEMENT SYSTEM
================================

1. Login
2. Exit

Enter choice:


After successful login, the user's role determines which dashboard is displayed.

### Admin Dashboard

================================
        ADMIN DASHBOARD
================================

1. Product Management
2. Category Management
3. Inventory Management
4. User Management
5. Reports
6. Logout


### Employee Dashboard

================================
      EMPLOYEE DASHBOARD
================================

1. View Products
2. Search Product
3. Check Stock
4. Stock In
5. Stock Out
6. Low Stock Products
7. Stock History
8. Logout

---

# 12. Example Backend Workflow — Stock Out

The Stock Out operation demonstrates the type of backend workflow that will be implemented.

### User Action

The employee selects:

5. Stock Out

The system asks:

Product ID:
101

Quantity:
5

### Backend Processing

Console/UI
    ↓
Inventory Service
    ↓
Validate quantity
    ↓
Check whether product exists
    ↓
Check current stock
    ↓
Check whether enough stock is available
    ↓
Calculate new stock
    ↓
Update product stock
    ↓
Create stock transaction
    ↓
Commit database transaction
    ↓
Return result to user

# 13. Initial Backend Architecture Concept

The application will gradually be developed using layered architecture.

                CONSOLE / UI
                     │
                     ▼
               SERVICE LAYER
                     │
                     ▼
                 DAO LAYER
                     │
                     ▼
                    JDBC
                     │
                     ▼
                  MYSQL

### Console/UI

Responsible for:

* Displaying menus.
* Accepting user input.
* Displaying results.

### Service Layer

Responsible for:

* Business rules.
* Validation.
* Coordinating operations.
* Deciding whether an operation is allowed.

### DAO Layer

Responsible for:

* Database operations.
* SQL execution.
* Reading and writing data through JDBC.

### JDBC

Provides communication between Java and MySQL.

### MySQL

Provides persistent storage for application data.

This architecture will be implemented gradually during later phases.

---

# 14. Phase 1 Development Approach

The project will not be developed by writing the entire application at once.

It will be built progressively:

Requirements
     ↓
Domain Design
     ↓
Database Design
     ↓
Core Java
     ↓
JDBC
     ↓
DAO
     ↓
Service
     ↓
Transactions
     ↓
Authentication
     ↓
Authorization
     ↓
Reports
     ↓
Testing
     ↓
Refactoring
     ↓
Spring Boot REST API


Each phase will introduce only the concepts needed at that stage.

---

# 15. Phase 1 Conclusion

Phase 1 establishes what the Inventory Management System is supposed to do.

At this stage, the project requirements define:

* The problem being solved.
* The objectives of the system.
* The scope of the application.
* The users and their roles.
* The major modules.
* The functional requirements.
* The non-functional requirements.
* The basic user workflow.
* The expected backend workflow.
* The initial layered architecture.

The next phase will convert these requirements into a concrete **domain model**.

We will identify the entities required by the system, their attributes, their relationships, and how real-world business objects will be represented as Java objects and eventually stored in the MySQL database.
