# Multi-Role Exam Management System (Laravel API + Web)

This is a mini online exam management system built with **Laravel**, supporting **Admin** and **Student** roles.

---

## Features

## Installed Commands & Libraries List

### Laravel Project Setup
- composer create-project laravel/laravel exam-system

# 1. Authentication
- composer require laravel/ui
- php artisan ui bootstrap --auth
- npm install && npm run dev

# 2. Run migrations
php artisan migrate

# 3. Install encryption keys and clients
php artisan passport:install


# 4. Database & Models
- php artisan make:migration add_role_to_users


### Admin
- CRUD for Categories, Subcategories, Subjects, and Questions (Web UI using Blade)
- MCQ Questions: 4 options + select correct answer
- True/False Questions: select correct boolean
- Only verified admins can login

### Student
- API endpoints to:
  - Fetch random questions for a selected subject (limit 10 per exam)
  - Submit exam answers and calculate score
  - View past exam attempts with details
- Only verified students can login

---

## Authentication & Email Verification

- Registration requires email verification via **OTP**.
- OTP is sent via email and stored in database.
- OTP expires in **10 minutes**.
- Users **cannot login** until email is verified.
- API token is generated only after verification.

---

## API Endpoints

### 1. Register
**POST** `/api/register`

**Request Body:**
```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "password_confirmation": "password123",
    "role": "admin"
}

**Response (Success)**
{
    "message": "Registration successful. Please verify your email with OTP.",
    "email": "john@example.com"
}


### 2. Verify OTP
**POST** `/api/verify-otp`

**Request Body**
```json
{
    "email": "john@example.com",
    "otp": "123456"
}

**Response (Success)**
{
    "message": "Email verified successfully"
}


### 3. . Login
**POST** `/api/login`

**Request Body**
```json
{
    "email": "john@example.com",
    "password": "password123"
}

**Response (Success)**
{
    "token": "your_api_token_here",
    "user": {
        "id": 1,
        "name": "John Doe",
        "email": "john@example.com",
        "role": "admin"
    }
}

**Response (Unverified Email)**
{
    "error": "Please verify your email first"
}

**Response (Invalid Credentials)**
{
    "error": "Invalid credentials"
}

**Database Structure**

1. users: id, name, email, password, role, otp, otp_expires_at, is_verified, api_token

2. categories: id, name, description

3. subcategories: id, category_id, name, description

4. subjects: id, subcategory_id, name, description

5. questions: id, subject_id, question_text, question_type, options (JSON), correct_answer

6. exam_attempts: id, user_id, subject_id, score, started_at, ended_at

7. exam_answers: id, exam_attempt_id, question_id, given_answer, is_correct



