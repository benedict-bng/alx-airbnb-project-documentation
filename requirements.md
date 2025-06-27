✅ 1. User Authentication
📌 Description:
Handles user registration, login, and role-based access.

🔐 API Endpoints:
POST /api/auth/register
Input:

json
Copy
Edit
{
  "first_name": "Jane",
  "last_name": "Doe",
  "email": "jane@example.com",
  "password": "securePass123",
  "phone_number": "0712345678",
  "role": "guest"
}
Validation Rules:

email: valid format, unique

password: min 8 characters, strong

role: one of guest, host, admin

Output:

json
Copy
Edit
{
  "message": "Registration successful",
  "user_id": "uuid",
  "token": "jwt_token"
}
POST /api/auth/login
Input:

json
Copy
Edit
{
  "email": "jane@example.com",
  "password": "securePass123"
}
Output:

json
Copy
Edit
{
  "message": "Login successful",
  "user": {
    "user_id": "uuid",
    "role": "guest"
  },
  "token": "jwt_token"
}
✅ Performance Criteria:
Login/registration response time: ≤ 500ms

Token expiry: configurable (e.g., 1h)

✅ 2. Property Management
📌 Description:
Allows hosts to list, update, and manage properties.

🏠 API Endpoints:
POST /api/properties
Input:

json
Copy
Edit
{
  "name": "Seaside Villa",
  "description": "A beautiful house near the beach",
  "location": "Diani, Kenya",
  "pricepernight": 120.00
}
Validation Rules:

name: required

pricepernight: > 0

Output:

json
Copy
Edit
{
  "message": "Property created",
  "property_id": "uuid"
}
GET /api/properties
Query Parameters (optional):

location=Kenya

min_price=50&max_price=200

availability=true

Output:

json
Copy
Edit
[
  {
    "property_id": "uuid",
    "name": "Seaside Villa",
    "location": "Diani",
    "pricepernight": 120.00
  },
  ...
]
PUT /api/properties/{id}
Updates a property’s details (host only).

DELETE /api/properties/{id}
Soft delete (mark as inactive).

✅ Performance Criteria:
Property search filters response ≤ 800ms

Support pagination (e.g., limit=10&offset=20)

✅ 3. Booking System
📌 Description:
Manages property bookings and availability.

📅 API Endpoints:
POST /api/bookings
Input:

json
Copy
Edit
{
  "property_id": "uuid",
  "start_date": "2025-07-10",
  "end_date": "2025-07-15"
}
Validation Rules:

start_date < end_date

Property must be available for the selected dates

Output:

json
Copy
Edit
{
  "message": "Booking created",
  "booking_id": "uuid",
  "total_price": 600.00,
  "status": "pending"
}
GET /api/bookings/user
Authenticated user sees their bookings.

PUT /api/bookings/{id}/cancel
Cancel a pending or confirmed booking.

✅ Performance Criteria:
Prevent double booking via DB constraint or locking

Booking confirmation latency ≤ 1s

