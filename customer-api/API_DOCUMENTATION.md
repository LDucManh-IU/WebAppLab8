# Customer API Documentation

## Base URL
```
http://localhost:8080/api/customers
```

---

## Endpoints

### 1. Get All Customers (Simple)
**GET** `/api/customers/all`

Get all customers without pagination or sorting.

**Response:** 200 OK
```json
[
    {
        "id": 1,
        "customerCode": "C001",
        "fullName": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1-555-0101",
        "address": "123 Main St, New York",
        "status": "ACTIVE",
        "createdAt": "2024-11-03T10:00:00"
    },
    {
        "id": 2,
        "customerCode": "C002",
        "fullName": "Jane Smith",
        "email": "jane.smith@example.com",
        "phone": "+1-555-0102",
        "address": "456 Oak Ave, Los Angeles",
        "status": "ACTIVE",
        "createdAt": "2024-11-03T10:05:00"
    }
]
```

---

### 2. Get All Customers (with Pagination & Sorting)
**GET** `/api/customers`

Get customers with optional pagination and sorting.

**Query Parameters:**
- `page` (optional): Page number (0-indexed)
- `size` (optional): Number of items per page
- `sortBy` (optional): Field to sort by (e.g., fullName, email, createdAt)
- `sortDir` (optional): Sort direction (asc or desc), default: asc

**Examples:**

**Pagination only:**
```
GET /api/customers?page=0&size=5
```

**Sorting only:**
```
GET /api/customers?sortBy=fullName&sortDir=asc
```

**Pagination + Sorting:**
```
GET /api/customers?page=0&size=10&sortBy=createdAt&sortDir=desc
```

**Response (with pagination):** 200 OK
```json
{
    "customers": [
        {
            "id": 1,
            "customerCode": "C001",
            "fullName": "John Doe",
            "email": "john.doe@example.com",
            "phone": "+1-555-0101",
            "address": "123 Main St, New York",
            "status": "ACTIVE",
            "createdAt": "2024-11-03T10:00:00"
        }
    ],
    "currentPage": 0,
    "totalItems": 5,
    "totalPages": 1
}
```

---

### 3. Get Customer by ID
**GET** `/api/customers/{id}`

Get a single customer by ID.

**Path Parameters:**
- `id`: Customer ID

**Example:**
```
GET /api/customers/1
```

**Response:** 200 OK
```json
{
    "id": 1,
    "customerCode": "C001",
    "fullName": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1-555-0101",
    "address": "123 Main St, New York",
    "status": "ACTIVE",
    "createdAt": "2024-11-03T10:00:00"
}
```

**Error Response:** 404 Not Found
```json
{
    "timestamp": "2024-11-03T10:30:00",
    "status": 404,
    "error": "Not Found",
    "message": "Customer not found with id: 999",
    "path": "/api/customers/999"
}
```

---

### 4. Create New Customer
**POST** `/api/customers`

Create a new customer.

**Request Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
    "customerCode": "C006",
    "fullName": "David Miller",
    "email": "david.miller@example.com",
    "phone": "+1-555-0106",
    "address": "999 Broadway, Seattle, WA 98101"
}
```

**Response:** 201 Created
```json
{
    "id": 6,
    "customerCode": "C006",
    "fullName": "David Miller",
    "email": "david.miller@example.com",
    "phone": "+1-555-0106",
    "address": "999 Broadway, Seattle, WA 98101",
    "status": "ACTIVE",
    "createdAt": "2024-11-03T11:00:00"
}
```

**Error Response:** 400 Bad Request (Validation Error)
```json
{
    "timestamp": "2024-11-03T11:05:00",
    "status": 400,
    "error": "Validation Failed",
    "message": "Invalid input data",
    "path": "/api/customers",
    "details": [
        "customerCode: Customer code must be 3-20 characters",
        "email: Invalid email format",
        "fullName: Full name is required"
    ]
}
```

**Error Response:** 409 Conflict (Duplicate)
```json
{
    "timestamp": "2024-11-03T11:10:00",
    "status": 409,
    "error": "Conflict",
    "message": "Email already exists: david.miller@example.com",
    "path": "/api/customers"
}
```

---

### 5. Update Customer (Full Update)
**PUT** `/api/customers/{id}`

Update all fields of an existing customer.

**Path Parameters:**
- `id`: Customer ID

**Request Body:**
```json
{
    "customerCode": "C006",
    "fullName": "David Miller Jr.",
    "email": "david.miller.jr@example.com",
    "phone": "+1-555-0107",
    "address": "1000 Broadway, Seattle, WA 98101"
}
```

**Response:** 200 OK
```json
{
    "id": 6,
    "customerCode": "C006",
    "fullName": "David Miller Jr.",
    "email": "david.miller.jr@example.com",
    "phone": "+1-555-0107",
    "address": "1000 Broadway, Seattle, WA 98101",
    "status": "ACTIVE",
    "createdAt": "2024-11-03T11:00:00"
}
```

---

### 6. Partial Update Customer
**PATCH** `/api/customers/{id}`

Update only specified fields of an existing customer.

**Path Parameters:**
- `id`: Customer ID

**Request Body (Only fields to update):**
```json
{
    "fullName": "David Miller Updated"
}
```

**Response:** 200 OK
```json
{
    "id": 6,
    "customerCode": "C006",
    "fullName": "David Miller Updated",
    "email": "david.miller.jr@example.com",
    "phone": "+1-555-0107",
    "address": "1000 Broadway, Seattle, WA 98101",
    "status": "ACTIVE",
    "createdAt": "2024-11-03T11:00:00"
}
```

---

### 7. Delete Customer
**DELETE** `/api/customers/{id}`

Delete a customer by ID.

**Path Parameters:**
- `id`: Customer ID

**Example:**
```
DELETE /api/customers/6
```

**Response:** 200 OK
```json
{
    "message": "Customer deleted successfully"
}
```

**Error Response:** 404 Not Found
```json
{
    "timestamp": "2024-11-03T12:00:00",
    "status": 404,
    "error": "Not Found",
    "message": "Customer not found with id: 999",
    "path": "/api/customers/999"
}
```

---

### 8. Search Customers
**GET** `/api/customers/search`

Search customers by keyword (searches in name, email, and customer code).

**Query Parameters:**
- `keyword`: Search keyword

**Example:**
```
GET /api/customers/search?keyword=john
```

**Response:** 200 OK
```json
[
    {
        "id": 1,
        "customerCode": "C001",
        "fullName": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1-555-0101",
        "address": "123 Main St, New York",
        "status": "ACTIVE",
        "createdAt": "2024-11-03T10:00:00"
    },
    {
        "id": 3,
        "customerCode": "C003",
        "fullName": "Bob Johnson",
        "email": "bob.johnson@example.com",
        "phone": "+1-555-0103",
        "address": "789 Pine Rd, Chicago",
        "status": "ACTIVE",
        "createdAt": "2024-11-03T10:10:00"
    }
]
```

---

### 9. Filter Customers by Status
**GET** `/api/customers/status/{status}`

Get customers by status.

**Path Parameters:**
- `status`: Customer status (ACTIVE or INACTIVE)

**Examples:**
```
GET /api/customers/status/ACTIVE
GET /api/customers/status/INACTIVE
```

**Response:** 200 OK
```json
[
    {
        "id": 1,
        "customerCode": "C001",
        "fullName": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1-555-0101",
        "address": "123 Main St, New York",
        "status": "ACTIVE",
        "createdAt": "2024-11-03T10:00:00"
    }
]
```

---

### 10. Advanced Search
**GET** `/api/customers/advanced-search`

Search customers with multiple optional criteria.

**Query Parameters (all optional):**
- `name`: Search by name (partial match)
- `email`: Search by email (partial match)
- `status`: Filter by status (exact match)

**Examples:**
```
GET /api/customers/advanced-search?name=john
GET /api/customers/advanced-search?email=example.com&status=ACTIVE
GET /api/customers/advanced-search?name=john&email=gmail
```

**Response:** 200 OK
```json
[
    {
        "id": 1,
        "customerCode": "C001",
        "fullName": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1-555-0101",
        "address": "123 Main St, New York",
        "status": "ACTIVE",
        "createdAt": "2024-11-03T10:00:00"
    }
]
```

---

## HTTP Status Codes

### Success Codes

| Status Code | Description | Used In |
|------------|-------------|---------|
| 200 OK | Request succeeded | GET, PUT, PATCH, DELETE |
| 201 Created | Resource created successfully | POST |

### Client Error Codes

| Status Code | Description | Example |
|------------|-------------|---------|
| 400 Bad Request | Invalid request data / Validation failed | Missing required fields, invalid format |
| 404 Not Found | Resource doesn't exist | Customer ID not found |
| 409 Conflict | Duplicate or conflicting data | Duplicate email or customer code |

### Server Error Codes

| Status Code | Description | Example |
|------------|-------------|---------|
| 500 Internal Server Error | Server-side error | Database connection failure |

---

## Validation Rules

### CustomerRequestDTO (POST/PUT)

| Field | Rules | Example |
|-------|-------|---------|
| customerCode | Required, 3-20 chars, pattern: `^C\d{3,}$` | "C001", "C12345" |
| fullName | Required, 2-100 chars | "John Doe" |
| email | Required, valid email format | "john@example.com" |
| phone | Optional, pattern: `^\+?[0-9]{10,20}$` | "+1-555-0101" |
| address | Optional, max 500 chars | "123 Main St" |
| status | Optional | "ACTIVE", "INACTIVE" |

### CustomerUpdateDTO (PATCH)

All fields are optional. Only non-null fields will be updated.

| Field | Rules | Example |
|-------|-------|---------|
| fullName | 2-100 chars | "John Doe Updated" |
| email | Valid email format | "john.updated@example.com" |
| phone | Pattern: `^\+?[0-9]{10,20}$` | "+1-555-9999" |
| address | Max 500 chars | "New Address" |
| status | Valid status value | "ACTIVE", "INACTIVE" |

---

## Error Response Format

All error responses follow this structure:

```json
{
    "timestamp": "2024-11-03T10:30:00",
    "status": 404,
    "error": "Not Found",
    "message": "Customer not found with id: 999",
    "path": "/api/customers/999",
    "details": []
}
```

**For validation errors (400), the `details` array contains specific field errors:**
```json
{
    "timestamp": "2024-11-03T10:30:00",
    "status": 400,
    "error": "Validation Failed",
    "message": "Invalid input data",
    "path": "/api/customers",
    "details": [
        "customerCode: Customer code must be 3-20 characters",
        "email: Invalid email format"
    ]
}
```

---

## Testing Examples

### Using cURL

```bash
# GET all customers
curl http://localhost:8080/api/customers

# GET customer by ID
curl http://localhost:8080/api/customers/1

# POST create customer
curl -X POST http://localhost:8080/api/customers \
  -H "Content-Type: application/json" \
  -d '{
    "customerCode": "C006",
    "fullName": "David Miller",
    "email": "david.miller@example.com",
    "phone": "+1-555-0106",
    "address": "999 Broadway, Seattle"
  }'

# PUT update customer
curl -X PUT http://localhost:8080/api/customers/6 \
  -H "Content-Type: application/json" \
  -d '{
    "customerCode": "C006",
    "fullName": "David Miller Jr.",
    "email": "david.miller.jr@example.com",
    "phone": "+1-555-0107",
    "address": "1000 Broadway, Seattle"
  }'

# PATCH partial update
curl -X PATCH http://localhost:8080/api/customers/6 \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "David Miller Updated"
  }'

# DELETE customer
curl -X DELETE http://localhost:8080/api/customers/6

# Search customers
curl "http://localhost:8080/api/customers/search?keyword=john"

# Filter by status
curl http://localhost:8080/api/customers/status/ACTIVE

# Pagination
curl "http://localhost:8080/api/customers?page=0&size=5"

# Sorting
curl "http://localhost:8080/api/customers?sortBy=fullName&sortDir=asc"

# Pagination + Sorting
curl "http://localhost:8080/api/customers?page=0&size=10&sortBy=createdAt&sortDir=desc"

# Advanced search
curl "http://localhost:8080/api/customers/advanced-search?name=john&status=ACTIVE"
```

---

## Notes

- All endpoints support CORS (Cross-Origin Resource Sharing)
- The API uses JSON for request and response bodies
- Timestamps are in ISO-8601 format
- Customer code is immutable (cannot be changed after creation)
- Email and customer code must be unique
- IDs are auto-generated
- All list endpoints return arrays, even if empty

---

## Version

API Version: 1.0  
Last Updated: November 2024
