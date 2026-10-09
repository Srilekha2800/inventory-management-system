# Phase 2 — Domain Model Design

## 1. Purpose

This document defines the domain model for the Inventory Management System. It identifies the core business entities, their attributes and responsibilities, their relationships, and the business rules that guide the design.

The domain model is based on the requirements established in Phase 1. It will serve as the foundation for Phase 3 — Database Design and subsequent implementation using Java 21, Core Java, JDBC, and MySQL.

## 2. Domain Modeling Approach

The domain model was derived from the system's functional requirements and business rules rather than by treating every noun or feature as an entity.

A concept was considered a potential entity when the system needed to store and manage information about it, identify it independently, or preserve records of its activity.

The following four core entities were selected:

* **Product:** Represents an item managed by the inventory system.
* **Category:** Groups related products.
* **User:** Represents a person who accesses the system.
* **StockTransaction:** Records an inventory quantity movement and the user responsible for it.

This model covers the current project scope while avoiding unnecessary complexity.

## 3. Core Entities and Attributes

### 3.1 Product

**Purpose:** Represents an inventory item whose details and available quantity are managed by the system.

| Attribute      |                 Description                      | ----------------------------------------------------------------------------- |
| `productId`    | Unique identifier for the product.               |
| `name`         | Name of the product.                             |
| `price`        | Unit price of the product; must not be negative. |
| `currentStock` | Current available quantity; must not be negative.|
| `reorderLevel` | Quantity threshold used to identify low-stock products; must not be negative.                                     |
| `categoryId`   | Reference to the category to which the product belongs.                                                            |
| `active`       | Indicates whether the product is active or deactivated.                                                        |

**Responsibilities:**

* Store product details.
* Support product creation, updating, searching, and deactivation.
* Maintain the current inventory quantity.
* Support low-stock identification and inventory valuation.

**Design rule:** Normal product-detail updates must not directly change `currentStock`. Stock changes must occur through controlled inventory operations so that the corresponding transaction history is maintained.

### 3.2 Category

**Purpose:** Groups products into meaningful classifications.

| Attribute     | Description                            |
| ------------- | -------------------------------------------------------- |
| `categoryId`  | Unique identifier for the category.    |
| `name`        | Category name; must be unique.         |
| `description` | Optional description of the category.  |
| `active`      | Indicates whether the category is active or  deactivated.                                             |

**Responsibilities:**

* Organize products into categories.
* Support category identification and management.
* Help users filter and search for products by category.

**Design rule:** A category should not be deactivated while active products still depend on it, unless those products are reassigned or deactivated first.

### 3.3 User

**Purpose:** Represents a person authorized to access the Inventory Management System.

| Attribute      | Description                                     |
---------------------------------------------------------------------
| `userId`       | Unique identifier for the user.                 |
| `username`     | Unique username used for login.                 |
| `passwordHash` | Secure hash of the user's password; never the plain-text password.                                               |
| `role`         | User role, restricted to `ADMIN` or `EMPLOYEE`. |
| `active`       | Indicates whether the user account is active.   |

**Responsibilities:**

* Store account and authentication information.
* Support login and account deactivation.
* Provide the role used for authorization decisions.
* Identify the user responsible for each stock transaction.

**Design decision:** Roles are represented as a restricted set of values rather than a separate `Role` entity. This is sufficient while the system supports only the two predefined roles.

### 3.4 StockTransaction

**Purpose:** Records each stock movement to provide an auditable history of inventory changes.

| Attribute       | Description                                |
--------------------------------------------------------------  
| `transactionId` | Unique identifier for the stock transaction. |
| `productId`     | Reference to the product affected by the transaction. |
| `userId`        | Reference to the user responsible for the transaction.|
| `type`          | Type of movement: `OPENING_STOCK`, `STOCK_IN`, or `STOCK_OUT`. |
| `quantity`      | Positive quantity associated with the movement. |
| `createdAt`     | Date and time when the transaction was recorded. |

**Responsibilities:**

* Record opening stock and subsequent stock movements.
* Identify the affected product and responsible user.
* Preserve stock movement history.
* Support inventory reporting and auditing.

**Design decisions:**

* `quantity` is always positive. The transaction type determines whether stock increases or decreases.
* `createdAt` is included because transaction chronology is necessary for history and reporting.
* Transaction records should be preserved rather than deleted during ordinary product or user deactivation.

## 4. Entity Relationships

### 4.1 Category and Product

**Relationship:** One-to-many.

* One category can contain zero or many products.
* Each product belongs to one category under the current model.
* `Product.categoryId` references `Category.categoryId`.

### 4.2 Product and StockTransaction

**Relationship:** One-to-many.

* One product can have zero or many stock transactions.
* Each stock transaction refers to exactly one product.
* `StockTransaction.productId` references `Product.productId`.

### 4.3 User and StockTransaction

**Relationship:** One-to-many.

* One user can perform zero or many stock transactions.
* Each stock transaction is associated with exactly one responsible user.
* `StockTransaction.userId` references `User.userId`.

### Relationship Summary

```text
Category  1 -------- 0..* Product

Product  1 -------- 0..* StockTransaction

User     1 -------- 0..* StockTransaction
```

These relationships describe the logical domain model. Their implementation as primary keys, foreign keys, and constraints will be defined in Phase 3.

## 5. Opening Stock Design

When a product is created, its initial stock quantity must be handled consistently.

* If the initial quantity is zero, the product can be created without an opening-stock transaction.
* If the initial quantity is greater than zero, the system must create an `OPENING_STOCK` transaction containing that quantity.
* The opening-stock transaction must reference the product and the user responsible for creating it.
* Product creation, initial stock assignment, and transaction recording must succeed or fail together.

For example, if an authorized user creates a product with an initial quantity of 20 units, the product's `currentStock` is set to 20 and an `OPENING_STOCK` transaction is recorded for 20 units.

This approach ensures that the initial inventory balance has a corresponding history record.

## 6. Core Business Rules and Constraints

1. Every product, category, user, and stock transaction must have a unique identifier.
2. Product names and descriptions may be updated, subject to validation.
3. Category names and usernames must be unique.
4. Product price, current stock, reorder level, and transaction quantity must not be negative; transaction quantity must be greater than zero.
5. The only supported user roles are `ADMIN` and `EMPLOYEE`.
6. Only authorized users may perform operations permitted by their roles.
7. Stock-out operations must not reduce current stock below zero.
8. Stock-in and stock-out operations must update product stock and record the corresponding transaction consistently.
9. Opening stock must be recorded as described in Section 5 whenever the initial quantity is greater than zero.
10. Deactivated users must not be able to log in or perform new operations.
11. Deactivated products must not be used for normal stock-in or stock-out operations.
12. Deactivation must not erase historical stock transactions.
13. A category must not be deactivated while active products still reference it, unless those products are reassigned or deactivated first.
14. `currentStock` must be changed through controlled inventory operations, not through ordinary product-detail updates.
15. Low-stock status is determined by comparing a product's current stock with its reorder level.

Database transactions and constraints will be designed in Phase 3 and implemented in the appropriate application and persistence layers.

## 7. Concepts Not Modeled as Separate Entities

The following concepts are intentionally excluded from the initial domain model:

| Concept         | Reason                                             |
------------------------------------------------------------------------
| Role            | The system uses two predefined role values; a separate entity is not currently necessary.                            |
| Login           | Login is an operation, not a persistent business entity.                                                                |
| Report          | Reports are generated from product and transaction data.                                                                  |
| Dashboard       | A user-interface feature rather than a core domain entity.                                                                |
| Low-stock alert | Low-stock status can be calculated from `currentStock` and `reorderLevel`.                                     |
| Supplier        | Supplier management is outside the current project scope.                                                                 |
| PurchaseOrder   | Purchase order processing is outside the current project scope.                                                         |

These decisions can be revisited if the project requirements expand.

## 8. Requirements Traceability

| Requirement from Phase 1                        | Domain model support|
 ------------------------------------------------------------------------
| Create, update, search, and deactivate products | Product  |
| Organize products by category                   | Category and Product relationship |
| User accounts and login                         | User |
| Role-based access control                       | User role and authorization rules |
| Record stock-in and stock-out operations        | StockTransaction |
| Track opening inventory                         | Product and `OPENING_STOCK` transaction |
| Prevent negative stock                          | Product stock rules and controlled inventory operations |
| Identify low-stock products                     | `currentStock` and `reorderLevel` |
| Calculate inventory value                       | Product price and current stock   |
| Generate stock movement reports                 | StockTransaction history |
| Preserve historical activity                    | StockTransaction references and deactivation rules  |

## 9. Design Assumptions and Future Considerations

The current domain model makes the following assumptions:

* Every product belongs to one category.
* User roles are fixed to `ADMIN` and `EMPLOYEE`.
* Every stock transaction is associated with one product and one responsible user.
* Stock transaction timestamps are required; product and user creation/update timestamps are deferred.
* Supplier management, purchase orders, and more advanced inventory workflows are outside the initial scope.

The following details will be finalized during database design and implementation:

* Exact database data types, precision, and nullability.
* Primary keys, foreign keys, unique constraints, and indexes.
* Whether product names must be unique.
* Exact authorization permissions for each role.
* The database schema and transaction boundaries for stock operations.
* Validation rules and error handling for invalid inventory requests.

These items do not prevent completion of the current domain model. They are implementation details or requirements that need further specification before coding.

## 10. Phase 2 Conclusion

Phase 2 establishes a domain model containing four core entities: `Product`, `Category`, `User`, and `StockTransaction`.

The model represents product management, category organization, user access, and traceable inventory movements while keeping the initial implementation manageable.

The entity responsibilities, attributes, relationships, and core business rules are now defined at the domain level. The next phase will translate this logical model into a relational database design for MySQL, including tables, primary keys, foreign keys, constraints, and other necessary schema decisions.
