# E-Commerce API Documentation

## Overview
This is a RESTful E-Commerce API built with ASP.NET Core 8.0, featuring JWT authentication, Entity Framework Core, and comprehensive CRUD operations for products, orders, carts, and user management.

## Base URL
```
https://localhost:7000/api
```

## Authentication
The API uses JWT Bearer token authentication. Include the token in the Authorization header:
```
Authorization: Bearer {your_jwt_token}
```

## Response Format
All API responses follow this structure:
```json
{
  "success": boolean,
  "message": "string",
  "data": object | array | null,
  "errors": ["string"]
}
```

## Error Handling
- **400 Bad Request**: Validation errors or invalid input
- **401 Unauthorized**: Missing or invalid JWT token
- **403 Forbidden**: Insufficient permissions
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server-side errors

---

## Authentication Endpoints

### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "Password123!",
  "firstName": "John",
  "lastName": "Doe"
}
```

### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "Password123!"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresAt": "2024-01-01T12:00:00Z",
    "user": {
      "id": "user-id",
      "email": "user@example.com",
      "firstName": "John",
      "lastName": "Doe"
    }
  }
}
```

### Get Current User
```http
GET /api/auth/me
Authorization: Bearer {token}
```

### Change Password
```http
POST /api/auth/change-password
Authorization: Bearer {token}
Content-Type: application/json

{
  "currentPassword": "OldPassword123!",
  "newPassword": "NewPassword123!"
}
```

### Get All Users (Admin Only)
```http
GET /api/auth/admin/all
Authorization: Bearer {admin_token}
```

---

## Products Endpoints

### Get Products (Public)
```http
GET /api/products?searchTerm=phone&categoryId=1&minPrice=100&maxPrice=1000&pageNumber=1&pageSize=10&sortBy=price&sortOrder=asc
```

**Query Parameters:**
- `searchTerm` (optional): Search in product name/description
- `categoryId` (optional): Filter by category
- `minPrice`/`maxPrice` (optional): Price range filter
- `inStock` (optional): Filter by stock availability
- `sortBy`: "name", "price", "created"
- `sortOrder`: "asc", "desc"
- `pageNumber`: Default 1
- `pageSize`: Default 10

### Get Product by ID (Public)
```http
GET /api/products/{id}
```

### Get All Products (Admin Only)
```http
GET /api/products/admin/all
Authorization: Bearer {admin_token}
```

### Create Product (Admin Only)
```http
POST /api/products
Authorization: Bearer {admin_token}
Content-Type: application/json

{
  "name": "New Product",
  "description": "Product description",
  "price": 99.99,
  "stockQuantity": 100,
  "categoryId": 1
}
```

### Update Product (Admin Only)
```http
PUT /api/products/{id}
Authorization: Bearer {admin_token}
Content-Type: application/json

{
  "name": "Updated Product",
  "description": "Updated description",
  "price": 89.99,
  "stockQuantity": 50,
  "categoryId": 1
}
```

### Delete Product (Admin Only)
```http
DELETE /api/products/{id}
Authorization: Bearer {admin_token}
```

---

## Categories Endpoints

### Get All Categories (Public)
```http
GET /api/categories
```

### Get Category by ID (Public)
```http
GET /api/categories/{id}
```

### Create Category (Admin Only)
```http
POST /api/categories
Authorization: Bearer {admin_token}
Content-Type: application/json

{
  "name": "New Category",
  "description": "Category description"
}
```

### Update Category (Admin Only)
```http
PUT /api/categories/{id}
Authorization: Bearer {admin_token}
Content-Type: application/json

{
  "name": "Updated Category",
  "description": "Updated description"
}
```

### Delete Category (Admin Only)
```http
DELETE /api/categories/{id}
Authorization: Bearer {admin_token}
```

---

## Cart Endpoints (Authenticated Users)

### Get User Cart
```http
GET /api/cart
Authorization: Bearer {token}
```

### Add Item to Cart
```http
POST /api/cart/add
Authorization: Bearer {token}
Content-Type: application/json

{
  "productId": 1,
  "quantity": 2
}
```

### Update Cart Item
```http
PUT /api/cart/items/{cartItemId}
Authorization: Bearer {token}
Content-Type: application/json

{
  "quantity": 3
}
```

### Remove Item from Cart
```http
DELETE /api/cart/items/{cartItemId}
Authorization: Bearer {token}
```

### Clear Cart
```http
DELETE /api/cart/clear
Authorization: Bearer {token}
```

### Get All Carts (Admin Only)
```http
GET /api/cart/admin/all
Authorization: Bearer {admin_token}
```

---

## Orders Endpoints (Authenticated Users)

### Get User Orders
```http
GET /api/orders?pageNumber=1&pageSize=10
Authorization: Bearer {token}
```

### Get Order by ID
```http
GET /api/orders/{id}
Authorization: Bearer {token}
```

### Create Order
```http
POST /api/orders
Authorization: Bearer {token}
Content-Type: application/json

{
  "shippingAddress": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zipCode": "10001",
    "country": "USA"
  },
  "paymentMethod": "CreditCard"
}
```

### Get All Orders (Admin Only)
```http
GET /api/orders/admin/all?pageNumber=1&pageSize=10
Authorization: Bearer {admin_token}
```

### Update Order Status (Admin Only)
```http
PUT /api/orders/{id}/status
Authorization: Bearer {admin_token}
Content-Type: application/json

{
  "status": "Shipped"
}
```

---

## Data Models

### User
```json
{
  "id": "user-id",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "createdAt": "2024-01-01T00:00:00Z",
  "roles": ["Customer"]
}
```

### Product
```json
{
  "id": 1,
  "name": "Product Name",
  "description": "Product description",
  "price": 99.99,
  "stockQuantity": 100,
  "categoryId": 1,
  "categoryName": "Electronics",
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-01-01T00:00:00Z"
}
```

### Category
```json
{
  "id": 1,
  "name": "Electronics",
  "description": "Electronic devices and gadgets",
  "createdAt": "2024-01-01T00:00:00Z",
  "productCount": 5
}
```

### Cart
```json
{
  "id": 1,
  "userId": "user-id",
  "items": [
    {
      "id": 1,
      "productId": 1,
      "productName": "Product Name",
      "price": 99.99,
      "quantity": 2,
      "subtotal": 199.98
    }
  ],
  "totalAmount": 199.98,
  "itemCount": 2
}
```

### Order
```json
{
  "id": 1,
  "userId": "user-id",
  "orderNumber": "ORD-2024-001",
  "status": "Pending",
  "totalAmount": 199.98,
  "shippingAddress": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zipCode": "10001",
    "country": "USA"
  },
  "items": [
    {
      "productId": 1,
      "productName": "Product Name",
      "price": 99.99,
      "quantity": 2,
      "subtotal": 199.98
    }
  ],
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-01-01T00:00:00Z"
}
```

---

## Setup Instructions

### Prerequisites
- .NET 8.0 SDK
- SQL Server or SQLite
- Visual Studio 2022 or VS Code

### Configuration
1. Update `appsettings.json` with your database connection string
2. Update JWT settings in `appsettings.json`:
   ```json
   {
     "Jwt": {
       "Key": "YourSuperSecretKeyHereThatIsAtLeast32CharactersLong",
       "Issuer": "ECommerceAPI",
       "Audience": "ECommerceClient",
       "ExpireHours": "24"
     }
   }
   ```

### Running the API
```bash
cd ApiProject
dotnet restore
dotnet ef database update
dotnet run
```

### Default Admin Account
- Email: `admin@ecommerce.com`
- Password: `Admin123!`

---

## Rate Limiting & Security
- JWT tokens expire after 24 hours
- Password requirements: 6+ characters, uppercase, lowercase, digit
- Input validation using FluentValidation
- Global exception handling middleware
- CORS configuration for cross-origin requests

---

## Testing
Use the provided `ApiProject.http` file for testing endpoints in VS Code or import into Postman.

## Support
For technical issues, check the application logs or contact the development team. 