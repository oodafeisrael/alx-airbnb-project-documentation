# Backend Feature Specifications for Airbnb Clone

This document outlines the technical and functional requirements for three core backend features of the Airbnb Clone: **User Authentication**, **Property Management**, and **Booking System**.

---

##  1. User Authentication

###  API Endpoints

- `POST /api/auth/register` – Register a new user
- `POST /api/auth/login` – Authenticate user and issue JWT token
- `GET /api/auth/profile` – Retrieve current user profile (requires token)
- `PATCH /api/auth/profile` – Update user profile information

###  Input/Output Specifications

#### Register
**Input:**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "securePass123",
  "role": "guest"
}

**Output:**
{
  "message": "User registered successfully",
  "user_id": "abc123"
}

#### Login:
**Input:**
{
  "email": "jane@example.com",
  "password": "securePass123"
}

**Output:**
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "user_id": "abc123",
    "email": "jane@example.com",
    "role": "guest"
  }
}


### Validation Rules
- Email must be valid and unique
- Password must be at least 8 characters
- Role must be one of: guest, host, admin
- No duplicate accounts with same email

###  Performance Criteria
- Login/Register response time ≤ 500ms
- JWT expiration: 24 hours
- Profile fetch ≤ 300ms

## 2. Property Management
###  API Endpoints
- POST /api/properties – Create new property (Host only)
- GET /api/properties – List all available properties
- GET /api/properties/:id – Get a specific property by ID
- PATCH /api/properties/:id – Update property info
- DELETE /api/properties/:id – Remove a property
**Input/Output Specifications**
- Add Property
**Input:**
{
  "title": "Luxury Apartment in Lekki",
  "description": "Spacious, fully-furnished 2BR apartment",
  "price": 50000,
  "location": "Lekki, Lagos",
  "amenities": ["Wi-Fi", "Parking", "Air Conditioning"],
  "availability_start": "2025-07-01",
  "availability_end": "2025-08-31"
}
**Output:**
{
  "message": "Property listed successfully",
  "property_id": "xyz456"
}

### Validation Rules
- Title, description, and location must not be empty
- Price must be a positive number
- Availability dates must be valid and not overlap with existing bookings
- Only users with host role can add properties
⚙️### Performance Criteria
- Property listing load time ≤ 1.5 seconds
- Support pagination (default 10 per page)
- Property creation ≤ 800ms
###  3. Booking System
 ### API Endpoints
- POST /api/bookings – Create a booking
- GET /api/bookings – View all bookings (filter by user or host)
- PATCH /api/bookings/:id/cancel – Cancel a booking
- GET /api/bookings/:id – View a single booking
 **Input/Output Specifications**
### Create Booking
**Input:**

{
  "property_id": "xyz456",
  "start_date": "2025-07-05",
  "end_date": "2025-07-10"
}

**Output:**
{
  "message": "Booking confirmed",
  "booking_id": "bk789",
  "status": "confirmed"
}

### Validation Rules
- Dates must not overlap with existing bookings
- Property must be available during selected range
- Only users with guest role can make bookings
- End date must be after start date
⚙️### Performance Criteria
- Booking availability check ≤ 700ms
- Booking creation ≤ 1 second
- Booking cancellation ≤ 500ms

### Notes
- All endpoints must return appropriate HTTP status codes (200, 201, 400, 401, 403, 404)
- Authentication is required for all write operations (POST, PATCH, DELETE)
- Use PostgreSQL with proper indexing on user ID, property ID, and booking dates
