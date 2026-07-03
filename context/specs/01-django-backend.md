# Unit 01: Django Backend Initialization & Core User Models

## Goal

Configure the existing Django backend with a Custom User Model using `email` as the primary identifier, and verify that the local PostgreSQL database connection is properly mapped using `python-decouple`.

## Design

**Structural Decisions:**
- The project is already scaffolded in the `backend/` directory.
- We will create a dedicated `users` app to encapsulate authentication and profile data.
- The default Django `User` model will be replaced by a `CustomUser` extending `AbstractUser`.
- The `username` field will be removed entirely to enforce Email-based login, matching modern SaaS standards as required for the CareerSync platform.
- We will add an `is_premium` boolean to track subscription status for later integration with PayMongo.

## Implementation

### 1. App Initialization
- Run `python manage.py startapp users` inside the `backend/` directory.
- Register `'users'` within the `INSTALLED_APPS` list in `backend/settings.py`.

### 2. Custom User Model (`users/models.py`)
- Create `CustomUser(AbstractUser)`.
- Set `username = None`.
- Set `email = models.EmailField(unique=True)`.
- Add `is_premium = models.BooleanField(default=False)`.
- Set `USERNAME_FIELD = 'email'`.
- Set `REQUIRED_FIELDS = []`.

### 3. Custom User Manager (`users/managers.py`)
- Implement a `CustomUserManager(BaseUserManager)` to handle `create_user` and `create_superuser` correctly without requiring a username.

### 4. Settings Configuration (`backend/settings.py`)
- Add `AUTH_USER_MODEL = 'users.CustomUser'`.
- Verify the `DATABASES` configuration is properly importing from the `.env` file using `python-decouple` (e.g., `config('DB_NAME')`, `config('DB_USER')`, etc.).

### 5. Database Migrations
- Generate migrations for the new `users` app.
- Apply the migrations to the local PostgreSQL database to ensure the schema is instantiated.

## Dependencies

- *Assuming `python-decouple` and `psycopg2` are already present in the environment.* 

## Verify when done

- [ ] The `users` app is successfully created.
- [ ] `manage.py makemigrations` completes with no errors.
- [ ] `manage.py migrate` applies successfully to the local PostgreSQL database.
- [ ] You can create a superuser from the terminal using an email address (no username prompt).
- [ ] The Django development server (`manage.py runserver`) runs without errors.
