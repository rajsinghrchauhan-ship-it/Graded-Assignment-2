# Database Schema Documentation

## 1. Entity–Relationship Description

### ENTITY: customers
**Purpose:** Stores customer master information.

**Attributes:**
- `customer_id` (PK): Unique identifier for each customer
- `first_name`: Customer first name
- `last_name`: Customer last name
- `email`: Customer email address
- `phone`: Customer contact number
- `country`: Customer country
- `created_date`: Record creation date

**Relationships:**
- One customer can place **many orders** (1:M relationship with `orders` table)

---

### ENTITY: products
**Purpose:** Stores product catalog details.

**Attributes:**
- `product_id` (PK): Unique product identifier
- `product_name`: Name of the product
- `category`: Product category
- `price`: Unit price

**Relationships:**
- One product can appear in **many order items** (1:M with `order_items`)

---

### ENTITY: orders
**Purpose:** Stores customer order headers.

**Attributes:**
- `order_id` (PK): Unique order identifier
- `customer_id` (FK): References `customers.customer_id`
- `order_date`: Date of order
- `total_amount`: Total order value
- `status`: Order status

**Relationships:**
- One order belongs to **one customer**
- One order can contain **many order items** (1:M with `order_items`)

---

### ENTITY: order_items
**Purpose:** Stores line-level order details.

**Attributes:**
- `order_id` (FK): References `orders.order_id`
- `product_id` (FK): References `products.product_id`
- `quantity`: Quantity ordered
- `unit_price`: Price per unit

**Relationships:**
- Many order items belong to **one order**
- Many order items reference **one product**

---

## 2. Normalization Explanation (3NF)

The database schema follows **Third Normal Form (3NF)** principles to ensure data integrity and eliminate redundancy. Each table represents a single entity, and all non-key attributes are fully functionally dependent on the primary key. In the `customers` table, attributes such as name, email, and phone depend solely on `customer_id`. Similarly, in the `products` table, product attributes depend only on `product_id`.

Functional dependencies are clearly defined: `customer_id → customer attributes`, `product_id → product attributes`, and `order_id → order attributes`. The `order_items` table resolves the many-to-many relationship between orders and products, ensuring atomicity of data.

This design avoids **update anomalies** by preventing duplicated customer or product information across tables. **Insert anomalies** are avoided because new customers or products can be added independently of orders. **Delete anomalies** are prevented because deleting an order does not remove customer or product master data. By separating concerns into well-structured tables and enforcing foreign key relationships, the schema maintains consistency, scalability, and reliability.

---

## 3. Sample Data Representation

### customers
| customer_id | first_name | last_name | email | phone | country |
|------------|-----------|-----------|-------|-------|---------|
| 101 | Raj | Singh | raj@gmail.com | +919876543210 | India |
| 102 | Anita | Verma | anita@gmail.com | +919812345678 | India |

### products
| product_id | product_name | category | price |
|-----------|--------------|----------|-------|
| 201 | Laptop | Electronics | 55000 |
| 202 | Headphones | Accessories | 2500 |

### orders
| order_id | customer_id | order_date | total_amount | status |
|---------|-------------|------------|--------------|--------|
| 301 | 101 | 2025-01-10 | 57500 | Completed |
| 302 | 102 | 2025-01-12 | 2500 | Pending |

### order_items
| order_id | product_id | quantity | unit_price |
|---------|------------|----------|------------|
| 301 | 201 | 1 | 55000 |
| 301 | 202 | 1 | 2500 |

