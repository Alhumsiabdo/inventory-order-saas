# Problem Statement

## Business

A small mobile-accessories shop that sells phone chargers, headphones,
power banks, protective cases, and screen protectors.

The business has one owner and two sales employees.

## Current Problems

1. Employees record sales manually, so the owner cannot trust the current stock quantity.

2. Two employees can sell the final unit of the same product because they do not share one live stock record.

3. The owner cannot identify why a product quantity changed because there is no history of sales, new deliveries, damaged items, or corrections.

## Goal

The business needs one system where authorized staff can manage products,
record stock changes, create customer orders, and see accurate current stock.

Staff must be able to access only data belonging to their own business.

## Users and Roles

### Owner

The owner manages their business and has full access to data belonging to
their own business.

The owner can:
- Manage business settings.
- Create, edit, archive, and view products.
- Invite, remove, and manage employees.
- View all orders created in their business.
- View current stock and stock-change history.
- Record received stock, damaged items, missing items, and stock corrections.
- View simple sales and inventory reports.

The owner cannot:
- Access data belonging to another business.

### Sales Employee

A sales employee records sales for customers.

A sales employee can:
- View products and available stock in their own business.
- Create orders.
- View orders they created.
- View basic customer information needed to create an order.

A sales employee cannot:
- Manage employees.
- Change business settings.
- Correct stock manually.
- View or access data from another business.

### Inventory Employee

An inventory employee manages product quantities and stock records.

An inventory employee can:
- View products and stock quantities in their own business.
- Record newly received stock.
- Record damaged or missing stock.
- Create stock corrections with a reason.
- View stock-change history.

An inventory employee cannot:
- Manage employees.
- Change business settings.
- Access data from another business.

## Business Rules That Must Never Break

1. A user can view, create, update, or delete only data that belongs to their own business.

2. Only the owner can invite employees, remove employees, change employee roles,
   or modify business settings.

3. A confirmed order cannot contain a quantity greater than the available stock
   for any product.

4. Every stock quantity change must create a permanent stock-history record
   containing the product, quantity change, reason, date and time, and the user
   who made the change.

5. A sales employee cannot manually increase, decrease, or correct product stock.
   They can only change stock by creating or cancelling an order.

## Questions Still Unknown

- Should customers use the system directly, or only staff?
- Does the shop have one physical location or multiple locations?
- Does the business accept partial payments or only full payments?
