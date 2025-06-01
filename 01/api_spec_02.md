# Axyway API Specification

## Base Information

* **Base URL** : `https://api.axyway.com/v1`
* **Authentication** : Bearer Token
* **Content-Type** : `application/json`

## Common Response Structure

All API responses follow a consistent structure:

```json
{
  "status": "success|error",
  "code": 200,
  "message": "Response message",
  "data": {},
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_abc123"
}
```

## Endpoints

### 1. Get User Information

 **Endpoint** : `GET /users/{user_id}`

#### Request via APIM

```http
GET /v1/users/12345 HTTP/1.1
Host: api.axyway.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json
X-API-Key: your-api-key
```

#### Success Response (200)

```json
{
  "status": "success",
  "code": 200,
  "message": "User information retrieved successfully",
  "data": {
    "user_id": "12345",
    "username": "john_doe",
    "email": "john.doe@example.com",
    "profile": {
      "first_name": "John",
      "last_name": "Doe",
      "created_at": "2024-01-15T10:30:00Z",
      "last_login": "2025-06-01T08:45:00Z"
    },
    "preferences": {
      "timezone": "UTC",
      "language": "en"
    }
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_usr_001"
}
```

#### No Data Response (404)

```json
{
  "status": "error",
  "code": 404,
  "message": "User not found",
  "data": null,
  "error": {
    "type": "NOT_FOUND",
    "details": "No user exists with the provided user_id: 12345"
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_usr_002"
}
```

#### Server Error Response (500)

```json
{
  "status": "error",
  "code": 500,
  "message": "Internal server error occurred",
  "data": null,
  "error": {
    "type": "INTERNAL_SERVER_ERROR",
    "details": "Database connection timeout",
    "error_code": "DB_TIMEOUT_001"
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_usr_003"
}
```

### 2. Create New Order

 **Endpoint** : `POST /orders`

#### Request via APIM

```http
POST /v1/orders HTTP/1.1
Host: api.axyway.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json
X-API-Key: your-api-key

{
  "customer_id": "cust_789",
  "items": [
    {
      "product_id": "prod_001",
      "quantity": 2,
      "price": 29.99
    }
  ],
  "shipping_address": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip_code": "10001"
  }
}
```

#### Success Response (201)

```json
{
  "status": "success",
  "code": 201,
  "message": "Order created successfully",
  "data": {
    "order_id": "ord_456789",
    "customer_id": "cust_789",
    "status": "pending",
    "total_amount": 59.98,
    "currency": "USD",
    "items": [
      {
        "product_id": "prod_001",
        "product_name": "Wireless Headphones",
        "quantity": 2,
        "unit_price": 29.99,
        "total_price": 59.98
      }
    ],
    "created_at": "2025-06-01T12:00:00Z",
    "estimated_delivery": "2025-06-05T12:00:00Z"
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_ord_001"
}
```

#### No Data Response (400 - Invalid Request)

```json
{
  "status": "error",
  "code": 400,
  "message": "Invalid request data",
  "data": null,
  "error": {
    "type": "VALIDATION_ERROR",
    "details": "Required field 'customer_id' is missing",
    "validation_errors": [
      {
        "field": "customer_id",
        "message": "This field is required"
      },
      {
        "field": "items",
        "message": "At least one item is required"
      }
    ]
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_ord_002"
}
```

#### Server Error Response (503)

```json
{
  "status": "error",
  "code": 503,
  "message": "Service temporarily unavailable",
  "data": null,
  "error": {
    "type": "SERVICE_UNAVAILABLE",
    "details": "Payment processing service is currently down",
    "error_code": "SVC_DOWN_003",
    "retry_after": 300
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_ord_003"
}
```

### 3. Search Products

 **Endpoint** : `GET /products/search`

#### Request via APIM

```http
GET /v1/products/search?q=wireless+headphones&category=electronics&limit=10&offset=0 HTTP/1.1
Host: api.axyway.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json
X-API-Key: your-api-key
```

#### Success Response (200)

```json
{
  "status": "success",
  "code": 200,
  "message": "Products retrieved successfully",
  "data": {
    "products": [
      {
        "product_id": "prod_001",
        "name": "Premium Wireless Headphones",
        "description": "High-quality noise-canceling headphones",
        "price": 129.99,
        "currency": "USD",
        "category": "electronics",
        "in_stock": true,
        "stock_quantity": 45
      },
      {
        "product_id": "prod_002",
        "name": "Budget Wireless Earbuds",
        "description": "Affordable wireless earbuds with good sound",
        "price": 39.99,
        "currency": "USD",
        "category": "electronics",
        "in_stock": true,
        "stock_quantity": 120
      }
    ],
    "pagination": {
      "total_count": 2,
      "limit": 10,
      "offset": 0,
      "has_more": false
    }
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_search_001"
}
```

#### No Data Response (200 - Empty Results)

```json
{
  "status": "success",
  "code": 200,
  "message": "No products found matching search criteria",
  "data": {
    "products": [],
    "pagination": {
      "total_count": 0,
      "limit": 10,
      "offset": 0,
      "has_more": false
    }
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_search_002"
}
```

#### Server Error Response (502)

```json
{
  "status": "error",
  "code": 502,
  "message": "Bad gateway - upstream service error",
  "data": null,
  "error": {
    "type": "BAD_GATEWAY",
    "details": "Product catalog service returned invalid response",
    "error_code": "UPSTREAM_002",
    "upstream_service": "product-catalog-service"
  },
  "timestamp": "2025-06-01T12:00:00Z",
  "request_id": "req_search_003"
}
```

## Error Codes Reference

| Code | Type                  | Description                                       |
| ---- | --------------------- | ------------------------------------------------- |
| 400  | Bad Request           | Invalid request format or missing required fields |
| 401  | Unauthorized          | Invalid or missing authentication token           |
| 403  | Forbidden             | Insufficient permissions for the requested action |
| 404  | Not Found             | Requested resource does not exist                 |
| 429  | Too Many Requests     | Rate limit exceeded                               |
| 500  | Internal Server Error | Unexpected server error                           |
| 502  | Bad Gateway           | Upstream service error                            |
| 503  | Service Unavailable   | Service temporarily down                          |

## Rate Limiting

* **Rate Limit** : 1000 requests per hour per API key
* **Headers** :
* `X-RateLimit-Limit`: Maximum requests allowed
* `X-RateLimit-Remaining`: Remaining requests in current window
* `X-RateLimit-Reset`: Timestamp when rate limit resets

## Authentication

All requests must include a valid Bearer token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

API keys should also be included in the `X-API-Key` header for additional security.
