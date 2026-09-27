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