
# Airbnb Clone – Backend Requirement Specifications

This document outlines the functional and technical requirements for core backend features of the Airbnb Clone project.

---

## 1. User Authentication

### 🧩 Functional Requirements
- Users must be able to register, log in, and log out securely.
- Authentication must support role-based access: `guest`, `host`, `admin`.

### 🔌 API Endpoints
| Method | Endpoint           | Description         |
|--------|--------------------|---------------------|
| POST   | `/api/register`    | Register a new user |
| POST   | `/api/login`       | Authenticate user   |
| POST   | `/api/logout`      | Invalidate session  |

### 📥 Input Specification
**POST /api/register**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "securepassword123"
}
```

### 📤 Output
```json
{
  "message": "User registered successfully",
  "user_id": "uuid-1234",
  "token": "jwt_token"
}
```

### ✅ Validation Rules
- Email must be unique.
- Password must be at least 8 characters.
- Required fields must not be null.

### 🚀 Performance Criteria
- Response time < 500ms for login/registration.
- Rate limit: Max 5 registration attempts per minute/IP.

---

## 2. Property Management

### 🧩 Functional Requirements
- Hosts can create, edit, and delete their properties.
- Guests can view and search properties.

### 🔌 API Endpoints
| Method | Endpoint             | Description              |
|--------|----------------------|--------------------------|
| POST   | `/api/properties`    | Create property          |
| GET    | `/api/properties`    | List all properties      |
| GET    | `/api/properties/:id`| Get single property      |
| PUT    | `/api/properties/:id`| Update property          |
| DELETE | `/api/properties/:id`| Delete property          |

### 📥 Input (Create Property)
```json
{
  "name": "Cozy Apartment",
  "description": "Modern 2BR apartment in Kigali",
  "location": "Kigali, Rwanda",
  "pricepernight": 75.00
}
```

### 📤 Output
```json
{
  "message": "Property created successfully",
  "property_id": "uuid-5678"
}
```

### ✅ Validation Rules
- `pricepernight` must be a positive decimal.
- `name`, `location`, and `description` are required.
- Only hosts can create/edit/delete properties.

### 🚀 Performance Criteria
- Search/filter functionality should return results in < 1s.
- Indexed `location` and `pricepernight` for efficient querying.

---

## 3. Booking System

### 🧩 Functional Requirements
- Guests can book a property for specific dates.
- System should check for availability and prevent double bookings.

### 🔌 API Endpoints
| Method | Endpoint            | Description       |
|--------|---------------------|-------------------|
| POST   | `/api/bookings`     | Create a booking  |
| GET    | `/api/bookings/:id` | View a booking    |
| GET    | `/api/bookings`     | List my bookings  |

### 📥 Input (Create Booking)
```json
{
  "property_id": "uuid-5678",
  "start_date": "2025-07-10",
  "end_date": "2025-07-15"
}
```

### 📤 Output
```json
{
  "message": "Booking created successfully",
  "booking_id": "uuid-9012",
  "total_price": 375.00
}
```

### ✅ Validation Rules
- `start_date` must be before `end_date`.
- Dates must not overlap with existing bookings.
- Booking status must be one of: pending, confirmed, canceled.

### 🚀 Performance Criteria
- Total price calculation should be automatic.
- Prevent race conditions during booking via database transactions.

---
