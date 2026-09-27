# Domain Model

## Entities

### Business

Represents one shop using the system.

Examples of information:
- Business name.
- Business owner.
- Settings.

### User

Represents a person who can sign in to the system.

Examples of information:
- Name.
- Email address.
- Password.
- Account status.

### Business Membership

Represents a user's role inside one business.

Examples of information:
- Business.
- User.
- Role: `owner`, `sales_employee`, or `inventory_employee`.
- Membership status.

Why it exists:

A user may belong to one or more businesses in the future, and their role
can be different in each business.

### Product

Represents one sellable item belonging to a business.

Examples of information:
- Business.
- Name.
- Selling price.
- Product status: active or archived.

### Stock Movement

Represents one event that changes the quantity of a product.

Examples of reasons:
- `opening_stock`
- `stock_received`
- `order_confirmed`
- `order_cancelled`
- `damaged`
- `missing`
- `correction`

Examples of information:
- Business.
- Product.
- Quantity change: positive or negative.
- Reason.
- Optional note.
- User who made the change.
- Related order, if the movement was caused by an order.
- Date and time.

### Customer

Represents a person who buys products from the business.

Examples of information:
- Business.
- Name.
- Phone number.
- Optional note.

### Order

Represents one sale made to a customer.

Examples of information:
- Business.
- Customer.
- Employee who created it.
- Status: `confirmed` or `cancelled`.
- Date and time.
- Total amount.

### Order Item

Represents one product and quantity inside an order.

Examples of information:
- Order.
- Product.
- Quantity.
- Unit price at the time of sale.
- Line total.

## Relationships

1. A Business has many Business Memberships.  
   A Business Membership belongs to one Business.

2. A User has many Business Memberships.  
   A Business Membership belongs to one User.

3. A Business has many Products.  
   A Product belongs to one Business.

4. A Business has many Customers.  
   A Customer belongs to one Business.

5. A Business has many Orders.  
   An Order belongs to one Business.

6. A Business has many Stock Movements.  
   A Stock Movement belongs to one Business.

7. An Order belongs to one Customer.  
   A Customer can have many Orders.

8. An Order is created by one User.  
   A User can create many Orders.

9. An Order has many Order Items.  
   An Order Item belongs to one Order.

10. An Order Item belongs to one Product.  
    A Product can appear in many Order Items.

11. A Product has many Stock Movements.  
    A Stock Movement belongs to one Product.

12. A Stock Movement is created by one User.  
    A User can create many Stock Movements.

13. A Stock Movement can optionally belong to one Order.  
    An Order can have many Stock Movements.