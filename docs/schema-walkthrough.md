# Schema Walkthrough

This document checks whether the database design can store the important
business events defined in the user stories.

## Scenario 1: Owner creates a business

The system creates:
- One `businesses` record.
- One `users` record, if the owner does not already have an account.
- One `business_memberships` record with role `owner` and status `active`.

## Scenario 2: Owner creates a product with opening stock

The system creates:
- One `products` record belonging to the owner's business.
- One `stock_movements` record with:
    - The same business.
    - The new product.
    - A positive `quantity_change`.
    - Reason: `opening_stock`.
    - The owner as `created_by_user_id`.

## Scenario 3: Inventory employee receives 10 chargers

The system creates:
- One `stock_movements` record with:
    - The business.
    - The charger product.
    - `quantity_change`: `+10`.
    - Reason: `stock_received`.
    - The inventory employee as `created_by_user_id`.
    - An optional supplier or invoice note.

## Scenario 4: Sales employee confirms an order

The system creates:
- One `orders` record with status `confirmed`.
- One or more `order_items` records.
- One negative `stock_movements` record for each ordered product:
    - `quantity_change`: negative order-item quantity.
    - Reason: `order_confirmed`.
    - The related order.
    - The employee who created the order.

The system must perform all these changes in one database transaction.

## Scenario 5: Owner cancels an order

The system updates:
- The relevant `orders` record:
    - Status becomes `cancelled`.
    - `cancelled_at` is set.
    - `cancelled_by_user_id` is set.
    - An optional cancellation reason is stored.

The system creates:
- One positive `stock_movements` record for each order item:
    - `quantity_change`: positive order-item quantity.
    - Reason: `order_cancelled`.
    - The related order.
    - The user who cancelled the order.

The system must perform all these changes in one database transaction.

## Questions Found During Review

- How will the system calculate available stock?
- How will it stop two employees from selling the final unit at the same time?
- How will it ensure that a product, customer, order, and stock movement
  all belong to the same business?
- Can a customer be optional for a walk-in sale?
- Do we need payments in version 1?