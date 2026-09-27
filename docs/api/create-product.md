# API Contract: Create Product

## Purpose

Creates a new product for the authenticated user's currently selected business.

If opening stock is greater than zero, the system also creates:
- One current product-stock balance record.
- One stock-movement record with reason `opening_stock`.

All database changes must succeed or fail together in one transaction.

## Endpoint

```http
POST /api/v1/products
```

## Authentication

The request requires an authenticated user.

Only a user with the `owner` role in the current business may create a product.

## Request Body

```json
{
  "name": "Fast Charger 20W",
  "selling_price": 12.50,
  "opening_stock": 10
}
```

## Request Fields

| Field | Type | Required | Rules |
|---|---:|---:|---|
| `name` | string | Yes | Minimum 1 character, maximum 255 characters, unique within the current business |
| `selling_price` | decimal | Yes | Must be greater than or equal to 0 |
| `opening_stock` | integer | Yes | Must be greater than or equal to 0 |

The client must not send `business_id`. The backend gets the current business from the authenticated user's tenant context.

The client must not send `available_quantity`. The backend creates the stock balance from `opening_stock`.

## Successful Response

Status:

```http
201 Created
```

Response body:

```json
{
  "data": {
    "id": 1,
    "name": "Fast Charger 20W",
    "selling_price": "12.50",
    "status": "active",
    "available_quantity": 10,
    "created_at": "2026-09-27T00:00:00Z"
  }
}
```

## Validation Error

Status:

```http
422 Unprocessable Content
```

Example response:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": [
      "The name field is required."
    ],
    "opening_stock": [
      "The opening stock must be at least 0."
    ]
  }
}
```

## Authentication Error

Status:

```http
401 Unauthorized
```

This response is returned when the request has no valid authenticated user.

## Authorization Error

Status:

```http
403 Forbidden
```

This response is returned when an authenticated user does not have the
`owner` role in the current business.

## Business Behavior

1. The backend determines the current business from the authenticated user.
2. The backend validates the request.
3. The backend creates the product for the current business.
4. The backend creates one `product_stocks` record with
   `available_quantity` equal to `opening_stock`.
5. If `opening_stock` is greater than zero, the backend creates one
   `stock_movements` record with:
    - A positive `quantity_change`.
    - Reason `opening_stock`.
    - The authenticated user as creator.
6. The backend returns the created product and its available stock.
7. If any write fails, the transaction rolls back all changes.