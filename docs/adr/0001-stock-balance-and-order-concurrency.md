# ADR-0001: Maintain Current Stock Balance and Prevent Overselling

## Status

Accepted

## Context

The system must keep a permanent audit history of every stock change.

The system must also show available stock quickly in product lists and during
order creation.

Two sales employees may attempt to confirm orders for the same product at
nearly the same time. The system must never confirm orders that cause stock
to become negative.

## Decision

The system will use two sources of stock information:

1. `stock_movements` stores the immutable history of every stock change.

2. `product_stocks.available_quantity` stores the current available quantity
   for each product and business.

Every stock-changing operation must run inside one database transaction.

Every stock-changing operation must:

1. Lock the required `product_stocks` row or rows for update.
2. Check that sufficient available quantity exists.
3. Update `product_stocks.available_quantity`.
4. Create the corresponding `stock_movements` record or records.
5. Commit all changes together, or roll back all changes if any step fails.

When confirming an order with multiple products, the system will lock product
stock rows in ascending `product_id` order to reduce deadlock risk.

If the database reports a deadlock or serialization failure, the application
will retry the complete transaction a limited number of times. If it still
fails, the system will return a safe error and make no partial changes.

## Consequences

### Positive

- Prevents overselling during concurrent order requests.
- Gives fast access to current available stock.
- Preserves a complete stock audit trail.
- Makes inventory failures easier to investigate.
- Keeps order confirmation and cancellation atomic.

### Negative

- Stock-changing code is more complex.
- Concurrent requests for the same product may wait briefly.
- The system must handle transaction retry behavior.
- The application must periodically verify that current balances match
  the sum of stock movements.