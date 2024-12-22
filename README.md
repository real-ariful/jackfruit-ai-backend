# jackfruit-ai-backend
A RESTful API backend built with Django REST Framework.

## Tech Stack

- Python 3.13+
- Django 4.2+
- Django REST Framework
- PostgreSQL
- Redis (for caching)
- Celery (for async tasks)
- JWT Authentication
- Swagger/OpenAPI Documentation

## Development Setup

### Prerequisites

- Python 3.13+
- PostgreSQL
- Redis

### Installation

1. Clone the repository
```bash
git clone https://github.com/real-ariful/jackfruit-ai-backend.git
cd jackfruit-ai-backend
```

2. Set up virtual environment
```bash
python -m venv env
source env/bin/activate  # Windows: .\env\Scripts\activate
```

3. Install dependencies
```bash
pip install -r requirements.txt
```

4. Set up environment variables
Create `.env` file in the root directory:
```env
DEBUG=True
SECRET_KEY=your-secret-key
DB_NAME=your_db_name
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_HOST=localhost
DB_PORT=5432
REDIS_URL=redis://localhost:6379
CORS_ALLOWED_ORIGINS=http://localhost:3000
```

5. Run migrations
```bash
python manage.py migrate
```

6. Create superuser
```bash
python manage.py createsuperuser
```

7. Start development server
```bash
python manage.py runserver
```

### Run as docker
docker-compose up --build

docker-compose exec web python manage.py migrate

docker-compose exec web python manage.py createsuperuser

# Start containers
docker-compose up

# Start in detached mode
docker-compose up -d

# Stop containers
docker-compose down

# View logs
docker-compose logs -f

# Shell into web container
docker-compose exec web bash

# Run Django commands
docker-compose exec web python manage.py [command]

### Project Structure
```
jackfruitaibackend/
├── apps/
│   ├── users/
│   │   ├── migrations/
│   │   ├── api/
│   │   │   ├── serializers.py
│   │   │   ├── views.py
│   │   │   └── urls.py
│   │   ├── models.py
│   │   └── tests.py
│   └── core/
│       ├── migrations/
│       ├── management/
│       ├── utils/
│       └── models.py
├── config/
│   ├── settings/
│   │   ├── base.py
│   │   ├── local.py
│   │   └── production.py
│   ├── urls.py
│   └── wsgi.py
├── requirements/
│   ├── base.txt
│   ├── local.txt
│   └── production.txt
└── manage.py
```

## API Documentation

API documentation is available at `/api/docs/` using Swagger UI.

### Authentication

The API uses JWT Authentication. To authenticate:

1. Obtain token:
```bash
POST /api/token/
{
    "email": "user@example.com",
    "password": "password"
}
```

2. Use token in header:
```
Authorization: Bearer <your_token>
```

### API Endpoints

#### Users
- `POST /api/users/` - Register new user
- `GET /api/users/me/` - Get current user
- `PATCH /api/users/me/` - Update current user

#### Authentication
- `POST /api/token/` - Obtain JWT token
- `POST /api/token/refresh/` - Refresh JWT token

## Database

### Database Setup

1. Create PostgreSQL database:

```sql
CREATE ROLE your_db_role;
```
```sql
CREATE DATABASE your_db_name;
```
```sql
ALTER ROLE your_db_role with encrypted password your_db_password;
```
```sql
ALTER ROLE your_db_role with login;
```

2. Update database settings in `.env`

3. Run migrations:
```bash
python manage.py migrate
```

### Database Schema
Include your database schema or diagram here.

## Testing

### Running Tests
```bash
# Run all tests
python manage.py test

# Run specific test file
python manage.py test apps.users.tests.test_views

# Run with coverage
coverage run manage.py test
coverage report
```

### Test Structure
```
apps/
└── users/
    └── tests/
        ├── test_models.py
        ├── test_views.py
        └── test_serializers.py
```

## Celery Tasks

### Setup Redis
1. Install Redis
2. Start Redis server
3. Update REDIS_URL in .env

### Running Celery
```bash
# Start Celery worker
celery -A config worker -l info

# Start Celery beat for periodic tasks
celery -A config beat -l info
```

## Deployment

### Production Setup

1. Update production settings:
```bash
cp .env.example .env.prod
# Edit .env.prod with production values
```

2. Collect static files:
```bash
python manage.py collectstatic
```

3. Set up Gunicorn:
```bash
gunicorn config.wsgi:application
```

### Docker Deployment
```bash
# Build image
docker build -t project-backend .

# Run container
docker run -p 8000:8000 project-backend
```

## Code Style

- Follow PEP 8 guidelines
- Use Black for code formatting
- Use isort for import sorting

```bash
# Format code
black .

# Sort imports
isort .

# Check linting
flake8
```

## Maintenance

### Backup
```bash
# Backup database
python manage.py dumpdata > backup.json

# Restore database
python manage.py loaddata backup.json
```

### Logging
Logs are stored in `logs/` directory:
- `debug.log` - Development logs
- `error.log` - Production error logs

## Security Measures

- CORS configuration
- Rate limiting
- SSL/TLS in production
- Password hashing using Argon2
- XSS protection
- CSRF protection

## Contributing

1. Fork repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Submit pull request

## Versioning

We use semantic versioning:
- MAJOR.MINOR.PATCH

Current version: 1.


References:

https://www.django-rest-framework.org/tutorial/quickstart/