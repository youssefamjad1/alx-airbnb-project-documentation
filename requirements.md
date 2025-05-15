
# Backend Requirements Specifications

## 1. User Authentication

### Functional Requirements
- Users should be able to register using email and password.
- Users can log in using either credentials or OAuth (Google, Facebook).
- Authentication must use secure tokens (e.g., JWT).

### API Endpoints
#### POST `/api/auth/register`
- **Input**:  
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "Password123!"
  }
  ```
- **Validation**:
  - Email must be unique and valid format.
  - Password must be at least 8 characters, include uppercase, lowercase, and a number.
- **Output**:
  ```json
  {
    "message": "User registered successfully",
    "token": "<JWT_TOKEN>"
  }
  ```

#### POST `/api/auth/login`
- **Input**:
  ```json
  {
    "email": "john@example.com",
    "password": "Password123!"
  }
  ```
- **Output**:
  ```json
  {
    "message": "Login successful",
    "token": "<JWT_TOKEN>"
  }
  ```

### Performance Criteria
- Token generation should complete in < 200ms.
- Passwords must be hashed using bcrypt with at least 10 salt rounds.

---

## 2. Property Management (for Hosts)

### Functional Requirements
- Hosts can create, update, and delete property listings.
- Listings include title, description, price, location, and availability dates.

### API Endpoints
#### POST `/api/listings`
- **Input**:
  ```json
  {
    "title": "Ocean View Apartment",
    "description": "A beautiful beachside apartment",
    "price": 150,
    "location": "Casablanca",
    "availableFrom": "2025-06-01",
    "availableTo": "2025-07-01"
  }
  ```
- **Validation**:
  - Price must be numeric and greater than 0.
  - Dates must be in ISO format and valid.
- **Output**:
  ```json
  {
    "message": "Listing created",
    "listingId": "abc123"
  }
  ```

#### PUT `/api/listings/:id`
- **Input**: Updated listing fields
- **Output**: Success message with updated data

#### DELETE `/api/listings/:id`
- **Output**:  
  ```json
  {
    "message": "Listing deleted successfully"
  }
  ```

### Performance Criteria
- Create/Update/Delete operations should be completed in < 300ms.
- API must support pagination for listing retrieval.

---

## 3. Booking System

### Functional Requirements
- Guests can book a property for specific dates.
- System must check availability before confirming booking.
- Hosts and guests should both receive booking confirmations.

### API Endpoints
#### POST `/api/bookings`
- **Input**:
  ```json
  {
    "listingId": "abc123",
    "guestId": "user789",
    "startDate": "2025-06-10",
    "endDate": "2025-06-15"
  }
  ```
- **Validation**:
  - Dates must be available.
  - Booking must be at least 1 night and not exceed 30 days.
- **Output**:
  ```json
  {
    "message": "Booking confirmed",
    "bookingId": "booking456"
  }
  ```

#### GET `/api/bookings/:id`
- Returns booking details for a user or host.

### Performance Criteria
- Booking confirmation must happen in < 500ms.
- Concurrent booking requests must not result in double booking (use locking or transactions).

---

### Notes
- All endpoints must be protected via authentication middleware (except registration/login).
- Use proper status codes (`200`, `201`, `400`, `401`, `404`, etc.) for all responses.
