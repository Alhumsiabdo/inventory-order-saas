## Folder Structure

```text
app/
├── Modules/
│   ├── Identity/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   ├── Requests/
│   │   │   └── Resources/
│   │   ├── Repositories/
│   │   ├── Services/
│   │   └── Providers/
│   │       └── IdentityServiceProvider.php
│   │
│   ├── Business/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   ├── Requests/
│   │   │   └── Resources/
│   │   ├── Repositories/
│   │   │   ├── BusinessRepositoryInterface.php
│   │   │   ├── BusinessRepository.php
│   │   │   ├── BusinessMembershipRepositoryInterface.php
│   │   │   └── BusinessMembershipRepository.php
│   │   ├── Services/
│   │   │   ├── BusinessServiceInterface.php
│   │   │   └── BusinessService.php
│   │   └── Providers/
│   │       └── BusinessServiceProvider.php
│   │
│   ├── Product/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   │   └── ProductController.php
│   │   │   ├── Requests/
│   │   │   │   └── StoreProductRequest.php
│   │   │   └── Resources/
│   │   │       └── ProductResource.php
│   │   ├── Repositories/
│   │   │   ├── ProductRepositoryInterface.php
│   │   │   └── ProductRepository.php
│   │   ├── Services/
│   │   │   ├── ProductServiceInterface.php
│   │   │   └── ProductService.php
│   │   └── Providers/
│   │       └── ProductServiceProvider.php
│   │
│   ├── Inventory/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   ├── Requests/
│   │   │   └── Resources/
│   │   ├── Repositories/
│   │   │   ├── ProductStockRepositoryInterface.php
│   │   │   ├── ProductStockRepository.php
│   │   │   ├── StockMovementRepositoryInterface.php
│   │   │   └── StockMovementRepository.php
│   │   ├── Services/
│   │   │   ├── InventoryServiceInterface.php
│   │   │   └── InventoryService.php
│   │   └── Providers/
│   │       └── InventoryServiceProvider.php
│   │
│   ├── Customer/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   ├── Requests/
│   │   │   └── Resources/
│   │   ├── Repositories/
│   │   │   ├── CustomerRepositoryInterface.php
│   │   │   └── CustomerRepository.php
│   │   ├── Services/
│   │   │   ├── CustomerServiceInterface.php
│   │   │   └── CustomerService.php
│   │   └── Providers/
│   │       └── CustomerServiceProvider.php
│   │
│   └── Order/
│       ├── Http/
│       │   ├── Controllers/
│       │   │   └── OrderController.php
│       │   ├── Requests/
│       │   │   └── StoreOrderRequest.php
│       │   └── Resources/
│       │       └── OrderResource.php
│       ├── Repositories/
│       │   ├── OrderRepositoryInterface.php
│       │   ├── OrderRepository.php
│       │   ├── OrderItemRepositoryInterface.php
│       │   └── OrderItemRepository.php
│       ├── Services/
│       │   ├── OrderServiceInterface.php
│       │   └── OrderService.php
│       └── Providers/
│           └── OrderServiceProvider.php
│
├── Models/
│   ├── Business.php
│   ├── BusinessMembership.php
│   ├── Customer.php
│   ├── Order.php
│   ├── OrderItem.php
│   ├── Product.php
│   ├── ProductStock.php
│   ├── StockMovement.php
│   └── User.php
│
├── Http/
│   └── Middleware/
│       └── ResolveCurrentBusiness.php
│
├── Policies/
│   ├── ProductPolicy.php
│   ├── OrderPolicy.php
│   └── StockMovementPolicy.php
│
└── Support/
    └── CurrentBusiness.php
```

## Service and Repository Rules

### Controllers

Controllers must be thin.

A controller must:
1. Receive a Form Request.
2. Authorize the request using a Policy.
3. Call a Service Interface.
4. Return a JsonResource or JSON response.

Controllers must not contain:
- Database queries.
- Transactions.
- Stock calculations.
- Business workflows.
- Direct creation of service or repository classes.

### Services

Services contain business logic and business workflows.

A Service may:
- Depend on one or more Repository Interfaces.
- Start and control database transactions.
- Call another service when necessary.
- Apply business rules.
- Coordinate multiple database changes.
- Throw domain/business exceptions.

A Service must not:
- Read directly from HTTP requests.
- Return HTTP responses.
- Contain controller behavior.

### Repositories

Repositories contain database queries only.

A Repository may:
- Query Eloquent models.
- Create, update, delete, or lock records.
- Apply query scopes, filters, sorting, and pagination.
- Return models, collections, paginators, or simple data objects.

A Repository must not:
- Contain business decisions.
- Start business workflows.
- Decide authorization.
- Return HTTP responses.
- Read request data directly.

### Interfaces

Every Service and Repository used by the application must have an interface.

Classes must depend on interfaces, not concrete implementations.

Interfaces are bound to concrete classes in the module Service Provider.

### Policies

Policies remain responsible for authorization.

Examples:
- Only an owner may create a product.
- Only an owner or sales employee may create an order.
- A sales employee may cancel only their own order.
- No user may access records from another business.

Services must not replace policies. The controller authorizes before calling a
service, and the service also receives the current business explicitly.

### Current Business

The current business must be resolved by middleware from an authenticated,
active business membership.

The client must not be trusted to provide `business_id` as tenant ownership
proof. Services receive the resolved current business or business ID from the
application layer.