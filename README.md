# Ticket Booking Backend (Bus Booking System)

A robust Django REST Framework backend for a bus ticket booking system featuring role-based authentication (User and Owner), JWT token management, and bus management APIs.

---

## Features

- **Authentication & Authorization**:
  - Role-based architecture (`user` and `owner`).
  - Public registration for regular users.
  - Restricted owner accounts requiring administrator approval before access.
  - Secure JWT authentication using `djangorestframework-simplejwt`.
- **Bus Management**:
  - List all buses with search and filter parameters (`source`, `destination`).
  - View individual bus details.
  - Create and manage bus routes and pricing.
- **Admin Control**:
  - Django Admin integration with 1-click owner approval actions.
- **Database**:
  - Configured for MySQL with PyMySQL adapter.
---

## Tech Stack

- **Framework**: Django 5.2 (LTS) & Django REST Framework 3.18
- **Authentication**: SimpleJWT 5.5
- **Database**: MySQL 8.0+
- **Language**: Python 3.13

---

## Project Structure

```text
ticket booking backend/
├── manage.py                          # Root proxy runner
├── requirements.txt                   # Project dependencies
├── .env.example                       # Sample environment configuration
├── database_schema.sql                # MySQL database schema & sample seed data
├── BusBooking_Postman_Collection.json # Postman API collection
└── BusBooking/
    ├── manage.py                      # Django management script
    ├── BusBooking/                    # Project settings & URL routing
    │   ├── settings.py
    │   ├── urls.py
    │   ├── wsgi.py
    │   └── asgi.py
    ├── buses/                         # Bus management app
    │   ├── models.py
    │   ├── serializers.py
    │   ├── views.py
    │   └── urls.py
    └── accounts/                      # Role-based auth & profile app
        ├── models.py
        ├── serializers.py
        ├── views.py
        ├── permissions.py
        └── urls.py
```

---

## Getting Started

### 1. Clone the repository
```bash
git clone git@github.com:TARUNVARMA765/ticket_booking_backend.git
cd ticket_booking_backend
```

### 2. Create and activate virtual environment
```bash
python -m venv venv
# Windows:
venv\Scripts\activate
# Linux/macOS:
source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Database Setup (MySQL)
1. Ensure MySQL server is running.
2. Create the database:
   ```sql
   CREATE DATABASE bus_booking_db;
   ```
3. Update database credentials in `BusBooking/BusBooking/settings.py` or `.env`.
4. Run migrations:
   ```bash
   python manage.py migrate
   ```

### 5. Run the Development Server
```bash
python manage.py runserver
```

---

## API Endpoints

### Authentication (`/api/accounts/`)
| Method | Endpoint | Description | Access |
|---|---|---|---|
| `POST` | `/api/accounts/register/` | Register regular user | Public |
| `POST` | `/api/accounts/login/` | User login (returns JWT) | Public |
| `POST` | `/api/accounts/owner/login/` | Owner login (requires admin approval) | Public (Owner credentials) |
| `POST` | `/api/accounts/token/refresh/` | Refresh JWT access token | Authenticated |
| `GET` | `/api/accounts/profile/` | Current user/owner profile | Authenticated |
| `GET` | `/api/accounts/user-only/` | User portal check | Role: User |
| `GET` | `/api/accounts/owner-only/` | Owner management portal check | Role: Approved Owner |
| `POST` | `/api/accounts/admin/create-owner/` | Create pre-approved owner | Admin only |

### Buses (`/api/buses/`)
| Method | Endpoint | Description | Query Params |
|---|---|---|---|
| `GET` | `/api/buses/` | List all buses | `?source=...&destination=...` |
| `POST` | `/api/buses/` | Create a new bus | - |
| `GET` | `/api/buses/<id>/` | Bus details | - |
| `PUT` | `/api/buses/<id>/` | Update bus details | - |
| `DELETE` | `/api/buses/<id>/` | Remove bus | - |

---

## Testing with Postman
Import `BusBooking_Postman_Collection.json` into Postman to run predefined test requests for registration, login, role checks, and bus APIs.
