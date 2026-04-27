# Smart Bike Rental Backend - Complete API Documentation

## Base URL
```
http://localhost:9080/api
```

## Table of Contents
1. [Authentication Endpoints](#authentication-endpoints)
2. [User Endpoints](#user-endpoints)
3. [Bike Endpoints](#bike-endpoints)
4. [Ride Endpoints](#ride-endpoints)
5. [Error Handling](#error-handling)
6. [Authentication Details](#authentication-details)

---

## Authentication Endpoints

### 1. User Registration
**Endpoint:** `POST /auth/register`

**Description:** Register a new user with email, password, name, and optional phone.

**Request Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!",
  "name": "John Doe",
  "phone": "+251912345678"
}
```

**Success Response (201 Created):**
```json
{
  "token": "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...",
  "type": "Bearer",
  "userId": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "role": "USER",
  "message": "User registered successfully"
}
```

**Error Response (400 Bad Request):**
```json
{
  "error": true,
  "message": "User with this email already exists"
}
```

---

### 2. User Login
**Endpoint:** `POST /auth/login`

**Description:** Authenticate user with email and password, returns JWT token.

**Request Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

**Success Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...",
  "type": "Bearer",
  "userId": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "role": "USER",
  "message": "Login successful"
}
```

**Error Response (401 Unauthorized):**
```json
{
  "error": true,
  "message": "Invalid email or password"
}
```

---

### 3. Validate Token
**Endpoint:** `POST /auth/validate`

**Description:** Validate if a JWT token is still valid.

**Request Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `token` (string, required): JWT token to validate

**Example Request:**
```
POST /auth/validate?token=eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...
```

**Success Response (200 OK):**
```json
{
  "valid": true,
  "message": "Token is valid"
}
```

**Invalid Response (200 OK):**
```json
{
  "valid": false,
  "message": "Token is invalid"
}
```

---

### 4. Refresh Token
**Endpoint:** `POST /auth/refresh`

**Description:** Generate a new JWT token using an existing valid token.

**Request Headers:**
```
Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...
```

**Success Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...",
  "type": "Bearer",
  "userId": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "role": "USER",
  "message": "Token refreshed successfully"
}
```

---

### 5. Get User Profile
**Endpoint:** `GET /auth/profile`

**Description:** Retrieve the authenticated user's profile information.

**Request Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `userId` (long, required): User ID

**Example Request:**
```
GET /auth/profile?userId=1
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "phone": "+251912345678",
  "role": "USER",
  "isActive": true,
  "createdAt": "2024-01-15T10:30:00",
  "updatedAt": "2024-01-15T10:30:00"
}
```

---

### 6. Update User Profile
**Endpoint:** `PUT /auth/profile`

**Description:** Update user profile information (name and phone).

**Request Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `userId` (long, required): User ID
- `name` (string, optional): Updated user name
- `phone` (string, optional): Updated phone number

**Example Request:**
```
PUT /auth/profile?userId=1&name=Jane%20Doe&phone=+251987654321
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "Jane Doe",
  "phone": "+251987654321",
  "role": "USER",
  "isActive": true,
  "createdAt": "2024-01-15T10:30:00",
  "updatedAt": "2024-01-15T11:00:00"
}
```

---

### 7. Change Password
**Endpoint:** `POST /auth/change-password`

**Description:** Change user's password after verification of old password.

**Request Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `userId` (long, required): User ID
- `oldPassword` (string, required): Current password
- `newPassword` (string, required): New password

**Example Request:**
```
POST /auth/change-password?userId=1&oldPassword=OldPass123!&newPassword=NewPass456!
```

**Success Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...",
  "type": "Bearer",
  "userId": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "role": "USER",
  "message": "Password changed successfully"
}
```

**Error Response (400 Bad Request):**
```json
{
  "error": true,
  "message": "Old password is incorrect"
}
```

---

## User Endpoints

### 1. Get User by ID
**Endpoint:** `GET /users/{userId}`

**Description:** Retrieve user information by user ID.

**Path Parameters:**
- `userId` (long, required): User ID

**Success Response (200 OK):**
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "phone": "+251912345678",
  "role": "USER",
  "isActive": true,
  "createdAt": "2024-01-15T10:30:00",
  "updatedAt": "2024-01-15T10:30:00"
}
```

**Error Response (404 Not Found):**
```json
{
  "status": 404,
  "message": "User not found"
}
```

---

### 2. Get User by Email
**Endpoint:** `GET /users/email/{email}`

**Description:** Retrieve user information by email address.

**Path Parameters:**
- `email` (string, required): User email address

**Example Request:**
```
GET /users/email/user@example.com
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "phone": "+251912345678",
  "role": "USER",
  "isActive": true,
  "createdAt": "2024-01-15T10:30:00",
  "updatedAt": "2024-01-15T10:30:00"
}
```

---

## Bike Endpoints

### 1. Create Bike
**Endpoint:** `POST /bikes`

**Description:** Add a new bike to the system.

**Request Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `bikeId` (string, required): Unique bike identifier
- `qrCode` (string, optional): QR code for the bike

**Example Request:**
```
POST /bikes?bikeId=BIKE001&qrCode=QR123456
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "bikeId": "BIKE001",
  "status": "LOCKED",
  "qrCode": "QR123456",
  "currentUser": null,
  "lastUpdated": "2024-01-15T10:30:00"
}
```

---

### 2. Get All Bikes
**Endpoint:** `GET /bikes`

**Description:** Retrieve all bikes in the system.

**Success Response (200 OK):**
```json
[
  {
    "id": 1,
    "bikeId": "BIKE001",
    "status": "LOCKED",
    "qrCode": "QR123456",
    "currentUser": null,
    "lastUpdated": "2024-01-15T10:30:00"
  },
  {
    "id": 2,
    "bikeId": "BIKE002",
    "status": "IN_USE",
    "qrCode": "QR123457",
    "currentUser": {
      "id": 1,
      "email": "user@example.com",
      "name": "John Doe"
    },
    "lastUpdated": "2024-01-15T11:00:00"
  }
]
```

---

### 3. Get Bike by ID
**Endpoint:** `GET /bikes/{bikeId}`

**Description:** Retrieve specific bike details.

**Path Parameters:**
- `bikeId` (string, required): Bike identifier

**Success Response (200 OK):**
```json
{
  "id": 1,
  "bikeId": "BIKE001",
  "status": "LOCKED",
  "qrCode": "QR123456",
  "currentUser": null,
  "lastUpdated": "2024-01-15T10:30:00"
}
```

---

### 4. Unlock Bike
**Endpoint:** `POST /bikes/{bikeId}/unlock`

**Description:** Unlock a bike via MQTT command to ESP32.

**Path Parameters:**
- `bikeId` (string, required): Bike identifier

**Success Response (200 OK):**
```json
{
  "message": "Unlock command sent"
}
```

---

### 5. Lock Bike
**Endpoint:** `POST /bikes/{bikeId}/lock`

**Description:** Lock a bike via MQTT command to ESP32.

**Path Parameters:**
- `bikeId` (string, required): Bike identifier

**Success Response (200 OK):**
```json
{
  "message": "Lock command sent"
}
```

---

## Ride Endpoints

### 1. Start Ride
**Endpoint:** `POST /rides/start`

**Description:** Start a new ride for a user with a specific bike (via QR code).

**Request Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `userId` (long, required): User ID
- `qrCode` (string, required): QR code of the bike

**Example Request:**
```
POST /rides/start?userId=1&qrCode=QR123456
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "user": {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe"
  },
  "bike": {
    "id": 1,
    "bikeId": "BIKE001",
    "status": "IN_USE",
    "qrCode": "QR123456"
  },
  "startTime": "2024-01-15T10:30:00",
  "endTime": null,
  "cost": 0.0,
  "active": true
}
```

**Error Response (400 Bad Request):**
```json
{
  "error": true,
  "message": "Bike is not available for rent"
}
```

---

### 2. End Ride
**Endpoint:** `POST /rides/end`

**Description:** End an active ride and calculate the cost.

**Request Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `rideId` (long, required): Ride ID

**Example Request:**
```
POST /rides/end?rideId=1
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "user": {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe"
  },
  "bike": {
    "id": 1,
    "bikeId": "BIKE001",
    "status": "LOCKED",
    "qrCode": "QR123456"
  },
  "startTime": "2024-01-15T10:30:00",
  "endTime": "2024-01-15T10:50:00",
  "cost": 10.0,
  "active": false
}
```

---

### 3. Get Ride Details
**Endpoint:** `GET /rides/{rideId}`

**Description:** Retrieve details of a specific ride.

**Path Parameters:**
- `rideId` (long, required): Ride ID

**Success Response (200 OK):**
```json
{
  "id": 1,
  "user": {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe"
  },
  "bike": {
    "id": 1,
    "bikeId": "BIKE001",
    "status": "LOCKED",
    "qrCode": "QR123456"
  },
  "startTime": "2024-01-15T10:30:00",
  "endTime": "2024-01-15T10:50:00",
  "cost": 10.0,
  "active": false
}
```

---

### 4. Get User Ride History
**Endpoint:** `GET /rides/user/{userId}`

**Description:** Retrieve all rides (past and active) for a specific user.

**Path Parameters:**
- `userId` (long, required): User ID

**Success Response (200 OK):**
```json
[
  {
    "id": 2,
    "user": {
      "id": 1,
      "email": "user@example.com",
      "name": "John Doe"
    },
    "bike": {
      "id": 2,
      "bikeId": "BIKE002",
      "status": "LOCKED",
      "qrCode": "QR123457"
    },
    "startTime": "2024-01-15T11:30:00",
    "endTime": "2024-01-15T11:45:00",
    "cost": 7.5,
    "active": false
  },
  {
    "id": 1,
    "user": {
      "id": 1,
      "email": "user@example.com",
      "name": "John Doe"
    },
    "bike": {
      "id": 1,
      "bikeId": "BIKE001",
      "status": "LOCKED",
      "qrCode": "QR123456"
    },
    "startTime": "2024-01-15T10:30:00",
    "endTime": "2024-01-15T10:50:00",
    "cost": 10.0,
    "active": false
  }
]
```

---

## Error Handling

### Common Error Responses

#### 400 Bad Request
```json
{
  "error": true,
  "message": "Invalid request parameters"
}
```

#### 401 Unauthorized
```json
{
  "error": true,
  "message": "Invalid email or password"
}
```

#### 404 Not Found
```json
{
  "error": true,
  "message": "Resource not found"
}
```

#### 500 Internal Server Error
```json
{
  "error": true,
  "message": "Internal server error. Please try again later."
}
```

---

## Authentication Details

### JWT Token Structure

Each JWT token contains:
- **Header:**
  ```json
  {
    "alg": "HS512",
    "typ": "JWT"
  }
  ```

- **Payload:**
  ```json
  {
    "sub": "user@example.com",
    "userId": 1,
    "role": "USER",
    "iat": 1705318200,
    "exp": 1705404600
  }
  ```

### Token Expiration
- **Default:** 24 hours (86400000 milliseconds)
- **Configurable in:** `application.yml` → `jwt.expiration`

### Using the Token

Include the token in the Authorization header for protected endpoints:
```
Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...
```

### Token Refresh Flow
1. When token is about to expire, use the `POST /auth/refresh` endpoint
2. Pass the existing token in the Authorization header
3. Receive a new token with extended expiration

---

## Cost Calculation

**Formula:** `Cost = Duration (minutes) × Price Per Minute`

**Current Rate:** 0.5 ETB per minute

**Examples:**
- 10 minutes ride = 5.0 ETB
- 20 minutes ride = 10.0 ETB
- 30 minutes ride = 15.0 ETB

---

## Database Schema

### Users Table
```sql
CREATE TABLE users (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  name VARCHAR(255) NOT NULL,
  phone VARCHAR(20),
  role VARCHAR(50) DEFAULT 'USER',
  is_active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Bikes Table
```sql
CREATE TABLE bikes (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  bike_id VARCHAR(255) UNIQUE NOT NULL,
  status VARCHAR(50) DEFAULT 'LOCKED',
  qr_code VARCHAR(255) UNIQUE,
  current_user_id BIGINT,
  last_updated TIMESTAMP,
  FOREIGN KEY (current_user_id) REFERENCES users(id)
);
```

### Rides Table
```sql
CREATE TABLE rides (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  user_id BIGINT NOT NULL,
  bike_id BIGINT NOT NULL,
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  cost DOUBLE,
  active BOOLEAN DEFAULT FALSE,
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (bike_id) REFERENCES bikes(id)
);
```

---

## Testing the API

### Using cURL

**Register User:**
```bash
curl -X POST http://localhost:9080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePassword123!",
    "name": "John Doe",
    "phone": "+251912345678"
  }'
```

**Login:**
```bash
curl -X POST http://localhost:9080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePassword123!"
  }'
```

**Validate Token:**
```bash
curl -X POST "http://localhost:9080/api/auth/validate?token=YOUR_JWT_TOKEN"
```

### Using Postman

1. Import the API endpoints into Postman
2. Set up environment variables for base URL and token
3. Use the token from login response in subsequent requests
4. Add `Authorization: Bearer {token}` header for protected endpoints

---

## Security Considerations

1. **Password Security:** All passwords are hashed using BCrypt algorithm
2. **JWT Secret:** Change the `jwt.secret` in production environment
3. **HTTPS:** Always use HTTPS in production
4. **Token Expiration:** Tokens expire after 24 hours by default
5. **CORS:** Configured to allow requests from all origins (update for production)
6. **Session Management:** Uses stateless JWT-based authentication

---

## Environment Configuration

Update `application.yml` for production:

```yaml
jwt:
  secret: "your-long-random-secret-key-min-32-chars"
  expiration: 86400000  # 24 hours

spring:
  datasource:
    url: "jdbc:mysql://your-db-host:3306/smartbike"
    username: "your-db-user"
    password: "your-db-password"
```

---

**Last Updated:** January 15, 2024
**Version:** 1.0.0
