# FastAPI Course Project

A complete FastAPI application with authentication, database integration, and deployment configuration.

## Features

- **FastAPI Framework** - Modern, fast web framework for building APIs
- **Authentication** - JWT-based authentication system
- **Database Integration** - PostgreSQL with SQLAlchemy ORM
- **Database Migrations** - Alembic for database schema management
- **File Uploads** - Support for file handling with aiofiles
- **API Documentation** - Automatic OpenAPI/Swagger documentation
- **Production Ready** - Nginx, Supervisor, and deployment configurations

## Tech Stack

- **Backend**: FastAPI, Python 3.8+
- **Database**: PostgreSQL
- **ORM**: SQLAlchemy 2.0
- **Authentication**: JWT (python-jose), bcrypt
- **Server**: Uvicorn (ASGI)
- **Reverse Proxy**: Nginx
- **Process Manager**: Supervisor
- **Database Migrations**: Alembic

## Prerequisites

- Python 3.8 or higher
- PostgreSQL 12+
- Git

## Local Development Setup

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/fastapi-course.git
cd fastapi-course
```

### 2. Create Virtual Environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Linux/macOS
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Set Up PostgreSQL Database

```sql
-- Connect to PostgreSQL as superuser
CREATE DATABASE fastapi;
CREATE USER fastapi_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE fastapi TO fastapi_user;
```

### 5. Environment Configuration

Create a `.env` file in the project root:

```env
DATABASE_HOSTNAME=localhost
DATABASE_PORT=5432
DATABASE_PASSWORD=your_password
DATABASE_NAME=fastapi
DATABASE_USERNAME=fastapi_user
SECRET_KEY=your_very_secure_secret_key_here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

Generate a secure secret key:
```bash
python -c "import secrets; print(secrets.token_urlsafe(32))"
```

### 6. Run Database Migrations

```bash
alembic upgrade head
```

### 7. Start the Development Server

```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

The API will be available at:
- **API**: http://localhost:8000
- **Documentation**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

## Project Structure

```
fastapi-course/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI application entry point
│   ├── config.py            # Configuration settings
│   ├── database.py          # Database connection setup
│   ├── models.py            # SQLAlchemy models
│   ├── schemas.py           # Pydantic schemas
│   ├── crud.py              # Database operations
│   ├── auth/                # Authentication modules
│   └── routers/             # API route handlers
├── alembic/                 # Database migration files
├── alembic.ini              # Alembic configuration
├── requirements.txt         # Python dependencies
├── .env                     # Environment variables (create this)
├── Procfile                 # Heroku deployment config
├── nginx                    # Nginx configuration
└── README.md
```

## API Endpoints

### Authentication
- `POST /users/` - Register new user
- `POST /login` - User login (returns JWT token)

### Users
- `GET /users/` - Get all users
- `GET /users/{id}` - Get user by ID
- `PUT /users/{id}` - Update user
- `DELETE /users/{id}` - Delete user

### Posts (Example)
- `GET /posts/` - Get all posts
- `POST /posts/` - Create new post
- `GET /posts/{id}` - Get post by ID
- `PUT /posts/{id}` - Update post
- `DELETE /posts/{id}` - Delete post

## Authentication

The API uses JWT (JSON Web Tokens) for authentication. Include the token in the Authorization header:

```bash
Authorization: Bearer your_jwt_token_here
```

Example API usage:
```bash
# Register a new user
curl -X POST "http://localhost:8000/users/" \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "securepass123"}'

# Login
curl -X POST "http://localhost:8000/login" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=user@example.com&password=securepass123"

# Use the returned token for authenticated requests
curl -X GET "http://localhost:8000/posts/" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

## Production Deployment

### Ubuntu Server Deployment

#### 1. Server Setup

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install dependencies
sudo apt install -y python3 python3-pip python3-venv git nginx postgresql postgresql-contrib supervisor
```

#### 2. Database Setup

```bash
sudo -u postgres psql
CREATE DATABASE fastapi;
CREATE USER fastapi_user WITH PASSWORD 'secure_production_password';
GRANT ALL PRIVILEGES ON DATABASE fastapi TO fastapi_user;
\q
```

#### 3. Application Setup

```bash
# Clone and setup project
cd /home/username
git clone https://github.com/yourusername/fastapi-course.git
cd fastapi-course

# Create virtual environment and install dependencies
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Create production .env file
# Run migrations
alembic upgrade head
```

#### 4. Nginx Configuration

```bash
sudo nano /etc/nginx/sites-available/fastapi
```

Add the provided nginx configuration, then:

```bash
sudo ln -s /etc/nginx/sites-available/fastapi /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```





