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