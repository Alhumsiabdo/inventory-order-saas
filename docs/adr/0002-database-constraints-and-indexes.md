# ADR-0002: Use Database Constraints and Query-Focused Indexes

## Status

Accepted

## Context

Application validation prevents many invalid requests, but validation alone is
not enough to protect data integrity.

The database must reject invalid data even if an application bug, a script, or
a direct database operation attempts to insert it.

The system is multi-tenant. Most application queries will filter records by
`business_id`, and some records will be filtered by business, status, user,
product, or date.

## Decision

The database schema will use the following constraints:

### Required values

Use `NOT NULL` for values that must always exist, including:
- Business names.
- User names, emails, and passwords.
- Foreign keys required by the business workflow.
- Product names and selling prices.
- Order status, total amount, and creator.
- Stock movement quantity, reason, product, business, and creator.

### Non-negative values

Use database check constraints to prevent:
- Negative product selling prices.
- Negative order totals.
- Negative current available stock.
- Zero or negative order-item quantities.

A stock movement quantity may be positive or negative because it represents a
change in stock, but it must never equal zero.

### Unique values

Use unique constraints for:
- `users.email`.
- One membership per `(business_id, user_id)`.
- One current stock balance per `product_id`.
- One product name per business in version 1.

### Foreign keys

Use foreign keys for all entity relationships defined in the ERD.

Foreign keys do not by themselves guarantee that every related record belongs
to the same business. The application must validate tenant ownership before
creating or modifying related records.

### Indexes

Add indexes based on expected queries, including:
- `products(business_id, status)`.
- `customers(business_id, name)`.
- `orders(business_id, status, created_at)`.
- `orders(created_by_user_id, created_at)`.
- `order_items(product_id)`.
- `stock_movements(business_id, product_id, created_at)`.
- `stock_movements(order_id)`.

Indexes will be reviewed using real query patterns and PostgreSQL query plans
after the application has data.

## Consequences

### Positive

- Invalid or inconsistent records are rejected close to the data.
- Query performance improves for common tenant-scoped screens.
- Database design documents the business rules explicitly.
- Future code has stronger protection against accidental bad writes.

### Negative

- Migrations become more detailed.
- Indexes add storage and make writes slightly more expensive.
- Constraints cannot replace application authorization or transaction logic.
- Unnecessary indexes can make write-heavy tables slower.