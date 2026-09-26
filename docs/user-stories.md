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