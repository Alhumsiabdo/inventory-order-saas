# User Stories

## US-001: Create a Product

### User Story

As an owner,  
I want to create a product with its basic information and opening stock,  
so that employees can see it and sell it correctly.

### Acceptance Criteria

1. The owner can enter a product name, selling price, and opening stock quantity.

2. The product name is required.

3. The selling price must be zero or greater.

4. The opening stock quantity must be zero or greater.

5. When the owner creates the product, it belongs only to the owner's business.

6. When the opening stock is greater than zero, the system creates a stock-history record with:
   - The product.
   - The quantity added.
   - The reason: `opening_stock`.
   - The date and time.
   - The owner who created the product.

7. A user from another business cannot view, create, update, or delete this product.

8. After the product is created, authorized employees in the same business can view its current available stock.

## US-002: Receive Stock

### User Story

As an owner or inventory employee,  
I want to record stock received for an existing product,  
so that the available stock stays accurate after new products arrive.

### Acceptance Criteria

1. Only an owner or inventory employee can record received stock.

2. The user must select a product that belongs to their own business.

3. The received quantity is required and must be greater than zero.

4. The user can optionally enter a note, such as a supplier name,
   invoice number, or delivery reference.

5. When stock is received, the system increases the product's available
   stock by the received quantity.

6. When stock is received, the system creates a permanent stock-history
   record containing:
   - The product.
   - The positive quantity added.
   - The reason: `stock_received`.
   - The optional note.
   - The date and time.
   - The user who recorded the stock.

7. A sales employee cannot record received stock.

8. A user cannot receive stock for a product belonging to another business.

## US-003: Create a Customer Order

### User Story

As a sales employee,  
I want to create a customer order containing one or more products,  
so that the shop can record a sale and reduce available stock correctly.

### Acceptance Criteria

1. Only an owner or sales employee can create an order.

2. The user can add one or more products belonging to their own business
   to an order.

3. Each order item must have a quantity greater than zero.

4. The system must use the product price stored on the server when creating
   the order. The client must not decide the final price.

5. Before confirming an order, the system checks that sufficient available
   stock exists for every requested product.

6. If the requested quantity is greater than the available stock for any
   product, the system rejects the order and does not change any stock.

7. When an order is confirmed, the system:
   - Creates the order.
   - Creates order items.
   - Reduces available stock for every ordered product.
   - Creates one stock-history record per ordered product.
   - Sets the stock-history reason to `order_confirmed`.
   - Records the employee who created the order.

8. Creating the order, reducing stock, and creating stock-history records
   must succeed or fail together. Partial changes are not allowed.

9. A user cannot create an order with products belonging to another business.

10. A sales employee can view orders they created, while the owner can view
    all orders belonging to their own business.

## US-004: Cancel an Order

### User Story

As an owner or sales employee,  
I want to cancel an order that was created by mistake or cannot be completed,  
so that the sale is not counted and its stock is returned correctly.

### Acceptance Criteria

1. Only an owner can cancel any order belonging to their own business.

2. A sales employee can cancel only an order they created and only before it
   is completed.

3. An order can be cancelled only when its current status is `confirmed`.

4. Cancelling an order must not delete the order or its order items.
   The system changes the order status to `cancelled`.

5. When an order is cancelled, the system increases available stock by the
   quantity of every item in that order.

6. When an order is cancelled, the system creates one stock-history record
   for each order item containing:
   - The product.
   - The positive quantity returned.
   - The reason: `order_cancelled`.
   - The related order.
   - The date and time.
   - The user who cancelled the order.

7. Cancelling an order, returning stock, and creating stock-history records
   must succeed or fail together. Partial changes are not allowed.

8. A cancelled order cannot be cancelled again.

9. A user cannot cancel an order belonging to another business.