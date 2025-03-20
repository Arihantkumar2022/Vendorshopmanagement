
---

# Vendor and Shop Management API

![Django](https://img.shields.io/badge/Django-4.2.7-green.svg)
![DRF](https://img.shields.io/badge/Django%20REST%20Framework-3.14.0-blue.svg)
![Python](https://img.shields.io/badge/Python-3.13-brightgreen.svg)

## Overview

The **Vendor and Shop Management API** is a Django-based RESTful API designed to manage vendors and their shops, with a focus on location-based functionality. It allows vendors to register, log in, manage their shops, and search for nearby shops based on geographical coordinates. The API uses JWT (JSON Web Tokens) for authentication and provides Swagger documentation for easy interaction.

This project is built using Django 4.2.7, Django REST Framework (DRF) 3.14.0, and `djangorestframework-simplejwt` for token-based authentication. It also integrates `drf-yasg` for API documentation and `haversine` for calculating distances between geographical coordinates.

---

## Features

- **Vendor Management**:
  - Register a new vendor with email, name, and password.
  - Log in to obtain JWT access and refresh tokens.
  - View and update vendor profile information.

- **Shop Management**:
  - Create, retrieve, update, and delete shops associated with a vendor.
  - Each shop includes a name, latitude, and longitude for location-based functionality.

- **Nearby Shop Search**:
  - Search for shops within a specified radius (in kilometers) from a given latitude and longitude using the Haversine formula.

- **Authentication**:
  - Token-based authentication using JWT (JSON Web Tokens).
  - Protected endpoints require a valid access token.

- **API Documentation**:
  - Interactive API documentation using Swagger UI (`/swagger/`) and Redoc (`/redoc/`).
  - Detailed endpoint descriptions and examples.

- **Admin Interface**:
  - Django admin panel for managing users, shops, and other data.

---

## Project Structure

```
vendorshopmanagement/
│
├── vendor_shop_system/       # Main project directory
│   ├── __init__.py
│   ├── settings.py           # Django settings
│   ├── urls.py              # Main URL configuration
│   └── wsgi.py              # WSGI entry point
│
├── accounts/                # App for vendor management
│   ├── migrations/          # Database migrations
│   ├── __init__.py
│   ├── admin.py             # Admin panel configuration
│   ├── models.py            # Vendor model
│   ├── serializers.py       # Serializers for vendor data
│   ├── urls.py              # URL routes for accounts app
│   └── views.py             # Views for vendor registration, login, and profile
│
├── shops/                   # App for shop management
│   ├── migrations/          # Database migrations
│   ├── __init__.py
│   ├── admin.py             # Admin panel configuration
│   ├── models.py            # Shop model
│   ├── serializers.py       # Serializers for shop data
│   ├── urls.py              # URL routes for shops app
│   └── views.py             # Views for shop CRUD and nearby search
│
├── api/                     # App for API routing
│   ├── __init__.py
│   ├── urls.py              # API URL routes
│   └── views.py             
│
├── static/                  # Static files
│
├── manage.py                # Django management script
├── requirements.txt         # Project dependencies
└── README.md                # Project documentation
```

---

## Prerequisites

Before setting up the project, ensure you have the following installed:

- **Python 3.13** (or a compatible version)
- **pip** (Python package manager)
- **Git** (for cloning the repository)
- **Virtualenv** (optional but recommended for isolating dependencies)

---

## Setup Instructions

Follow these steps to set up and run the project locally.

### 1. Clone the Repository
Clone the project repository to your local machine:

```bash
git clone https://github.com/your-username/vendorshopmanagement.git
cd vendorshopmanagement
```

### 2. Create a Virtual Environment
Create and activate a virtual environment to isolate project dependencies:

#### On Windows (PowerShell):
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

#### On Linux/macOS:
```bash
python3 -m venv venv
source venv/bin/activate
```

You should see `(venv)` in your terminal prompt, indicating the virtual environment is active.

### 3. Install Dependencies
Install the required Python packages listed in `requirements.txt`:

```bash
pip install -r requirements.txt
```

The `requirements.txt` file includes:

```
django==4.2.7
djangorestframework==3.14.0
djangorestframework-simplejwt==5.3.0
python-dotenv==1.0.0
django-cors-headers==4.3.0
haversine==2.8.0
drf-yasg==1.21.7
setuptools>=70.0.0
```

### 4. Apply Migrations
Set up the database by running migrations:
```bash
python manage.py makemigrations accounts shops
python manage.py migrate
```
This will create the necessary tables for the `accounts` and `shops` apps, as well as Django's built-in tables (e.g., for authentication).


### 5. Run the Development Server
Start the Django development server:

```bash
python manage.py runserver
```

The server will start at `http://127.0.0.1:8000/`. Open this URL in your browser to access the API.

---

## API Documentation

The API provides interactive documentation via Swagger UI and Redoc.

- **Swagger UI**: `http://127.0.0.1:8000/swagger/`
- **Redoc**: `http://127.0.0.1:8000/redoc/`

The root URL (`/`) redirects to the Swagger UI for convenience.

### Available Endpoints

#### Accounts
- **Register a Vendor**:
  - `POST /api/accounts/register/`
  - Request Body:
    ```json
    {
        "email": "vendor@example.com",
        "name": "Test Vendor",
        "password": "password123"
    }
    ```
  - Response (201 Created):
    ```json
    {
        "id": 1,
        "email": "vendor@example.com",
        "name": "Test Vendor"
    }
    ```

- **Login**:
  - `POST /api/accounts/login/`
  - Request Body:
    ```json
    {
        "email": "vendor@example.com",
        "password": "password123"
    }
    ```
  - Response (200 OK):
    ```json
    {
        "refresh": "your-refresh-token",
        "access": "your-access-token"
    }
    ```

- **Get Vendor Profile**:
  - `GET /api/accounts/profile/`
  - Headers: `Authorization: Bearer your-access-token`
  - Response (200 OK):
    ```json
    {
        "id": 1,
        "email": "vendor@example.com",
        "name": "Test Vendor"
    }
    ```

- **Update Vendor Profile**:
  - `PUT /api/accounts/profile/`
  - Headers: `Authorization: Bearer your-access-token`
  - Request Body:
    ```json
    {
        "name": "Updated Vendor Name"
    }
    ```
  - Response (200 OK):
    ```json
    {
        "id": 1,
        "email": "vendor@example.com",
        "name": "Updated Vendor Name"
    }
    ```

#### Shops
- **Create a Shop**:
  - `POST /api/shops/`
  - Headers: `Authorization: Bearer your-access-token`
  - Request Body:
    ```json
    {
        "name": "My Shop",
        "latitude": 28.7041,
        "longitude": 77.1025
    }
    ```
  - Response (201 Created):
    ```json
    {
        "id": 1,
        "name": "My Shop",
        "latitude": 28.7041,
        "longitude": 77.1025,
        "vendor": 1
    }
    ```

- **List Shops**:
  - `GET /api/shops/`
  - Headers: `Authorization: Bearer your-access-token`
  - Response (200 OK):
    ```json
    [
        {
            "id": 1,
            "name": "My Shop",
            "latitude": 28.7041,
            "longitude": 77.1025,
            "vendor": 1
        }
    ]
    ```

- **Retrieve a Shop**:
  - `GET /api/shops/{id}/`
  - Headers: `Authorization: Bearer your-access-token`
  - Response (200 OK):
    ```json
    {
        "id": 1,
        "name": "My Shop",
        "latitude": 28.7041,
        "longitude": 77.1025,
        "vendor": 1
    }
    ```

- **Update a Shop**:
  - `PUT /api/shops/{id}/`
  - Headers: `Authorization: Bearer your-access-token`
  - Request Body:
    ```json
    {
        "name": "Updated Shop Name",
        "latitude": 28.7041,
        "longitude": 77.1025
    }
    ```
  - Response (200 OK):
    ```json
    {
        "id": 1,
        "name": "Updated Shop Name",
        "latitude": 28.7041,
        "longitude": 77.1025,
        "vendor": 1
    }
    ```

- **Delete a Shop**:
  - `DELETE /api/shops/{id}/`
  - Headers: `Authorization: Bearer your-access-token`
  - Response (204 No Content)

#### Nearby Shops
- **Search Nearby Shops**:
  - `GET /api/nearby-shops/?latitude={lat}&longitude={lon}&radius={rad}`
  - Query Parameters:
    - `latitude`: Latitude of the reference point (e.g., 28.7041)
    - `longitude`: Longitude of the reference point (e.g., 77.1025)
    - `radius`: Radius in kilometers (e.g., 5)
  - Response (200 OK):
    ```json
    [
        {
            "id": 1,
            "name": "My Shop",
            "latitude": 28.7041,
            "longitude": 77.1025,
            "vendor": 1
        }
    ]
    ```

---

## Testing the API

You can test the API using the Swagger UI or tools like Postman or `curl`.

### Using Swagger UI
1. Open `http://127.0.0.1:8000/swagger/` in your browser.
2. Register a new vendor using the `POST /api/accounts/register/` endpoint.
3. Log in using the `POST /api/accounts/login/` endpoint to obtain a JWT token.
4. Click the "Authorize" button in Swagger, and enter `Bearer your-access-token` (replace `your-access-token` with the access token from the login response).
5. Test protected endpoints like `POST /api/shops/` or `GET /api/nearby-shops/`.

### Using Postman
1. **Register a Vendor**:
   - Method: `POST`
   - URL: `http://127.0.0.1:8000/api/accounts/register/`
   - Body (JSON):
     ```json
     {
         "email": "vendor@example.com",
         "name": "Test Vendor",
         "password": "password123"
     }
     ```

2. **Login**:
   - Method: `POST`
   - URL: `http://127.0.0.1:8000/api/accounts/login/`
   - Body (JSON):
     ```json
     {
         "email": "vendor@example.com",
         "password": "password123"
     }
     ```
   - Copy the `access` token from the response.

3. **Create a Shop**:
   - Method: `POST`
   - URL: `http://127.0.0.1:8000/api/shops/`
   - Headers: `Authorization: Bearer your-access-token`
   - Body (JSON):
     ```json
     {
         "name": "My Shop",
         "latitude": 28.7041,
         "longitude": 77.1025
     }
     ```

4. **Search Nearby Shops**:
   - Method: `GET`
   - URL: `http://127.0.0.1:8000/api/nearby-shops/?latitude=28.7041&longitude=77.1025&radius=5`

---

## Admin Panel

The Django admin panel is available at `http://127.0.0.1:8000/admin/`. Log in with the superuser credentials you created earlier to manage vendors, shops, and other data.

---

## Troubleshooting

- **ModuleNotFoundError: No module named 'pkg_resources'**:
  - Ensure `setuptools` is installed:
    ```bash
    pip install setuptools
    ```

- **404 Error on Root URL**:
  - Verify that the root URL (`/`) is configured in `vendor_shop_system/urls.py` to redirect to `/swagger/`.

- **Schema Generation Errors in Swagger**:
  - Check for errors in viewsets (e.g., `ShopViewSet`). Ensure `get_queryset` methods handle unauthenticated users and Swagger schema generation.

- **Static Files Not Loading**:
  - Run `python manage.py collectstatic` and ensure `STATIC_ROOT` and `STATICFILES_DIRS` are correctly configured in `settings.py`.

---
