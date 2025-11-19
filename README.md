# Job Board Platform Backend

A modern, scalable, passwordless/SSO-ready **Job Board Platform Backend** built with a modular architecture and production-grade standards.

---

## 🚀 Project Overview
This backend powers a job board system that supports:
- User authentication via **SSO/passwordless providers** (Google, GitHub, Email Magic Link, Phone OTP).
- Role-based access for admins and users.
- Company management.
- Job postings with advanced filtering.
- Job applications.
- Following companies and saving jobs.
- Swagger/OpenAPI documentation.

This project follows real-world backend development workflows including modular apps, optimized database queries, and clean API design.

---

## 🧩 Key Features

### **1. Authentication (SSO / Passwordless)**
- No password storage.
- Supports Google, GitHub, Apple, email link, phone OTP, etc.
- Users can link multiple auth providers.

### **2. Users & Profiles**
- Skill tagging system.
- Editable professional profile.

### **3. Companies**
- Create, update, and manage company profiles.
- Users can follow companies.

### **4. Job Postings**
- Admin/company accounts can create jobs.
- Filtering by location, employment type, remote, salary, etc.

### **5. Applications**
- Users can apply for jobs.
- Each user can only apply once per job.

### **6. Saved Jobs / Bookmarks**
- Users save jobs for quick access.

### **7. Swagger Documentation**
- Complete API docs at `/api/docs`.

---

## 🛠 Tech Stack

| Technology | Purpose |
|------------|----------|
| **Django** | Backend framework |
| **PostgreSQL** | Relational database |
| **JWT** | Authentication tokens |
| **Swagger / drf-yasg** | API documentation |
| **Docker** (optional) | Deployment |

---

## 🗂 Recommended Django Apps

| App | Responsibility |
|-----|----------------|
| `accounts` | Users, skills, SSO authentication |
| `companies` | Company profiles |
| `jobs` | Job postings, filtering |
| `applications` | Job applications |
| `core` | Permissions, helpers, utilities |
| `documentation` | Swagger/OpenAPI setup |

---

## 🗄 Database Schema Summary

### **Users (passwordless)**
- Stores only profile info and email.

### **Auth Providers**
- Stores provider type and external provider user ID.

### **Skills**
- Many-to-Many with users.

### **Companies**
- Companies posting jobs.

### **Jobs**
- Posted by companies.

### **Applications**
- One user → many applications.  
- One job → many applications.

### **Saved Jobs**
- Many-to-Many between users and jobs.

### **Company Followers**
- Many-to-Many between users and companies.

---

## 🔗 SQL Schema

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    headline VARCHAR(255),
    bio TEXT,
    location VARCHAR(255),
    profile_photo_url TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE auth_providers (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT REFERENCES users(id) ON DELETE CASCADE,
    provider VARCHAR(50) NOT NULL,
    provider_user_id VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    UNIQUE (provider, provider_user_id)
);

CREATE TABLE skills (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE user_skills (
    user_id BIGINT REFERENCES users(id) ON DELETE CASCADE,
    skill_id BIGINT REFERENCES skills(id) ON DELETE CASCADE,
    PRIMARY KEY (user_id, skill_id)
);

CREATE TABLE companies (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    website VARCHAR(255),
    logo_url TEXT,
    location VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE jobs (
    id BIGSERIAL PRIMARY KEY,
    company_id BIGINT REFERENCES companies(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    requirements TEXT,
    employment_type VARCHAR(50),
    location VARCHAR(255),
    salary_range VARCHAR(255),
    remote BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE applications (
    id BIGSERIAL PRIMARY KEY,
    job_id BIGINT REFERENCES jobs(id) ON DELETE CASCADE,
    user_id BIGINT REFERENCES users(id) ON DELETE CASCADE,
    resume_url TEXT,
    cover_letter TEXT,
    status VARCHAR(50) DEFAULT 'submitted',
    created_at TIMESTAMP DEFAULT NOW(),
    UNIQUE (job_id, user_id)
);

CREATE TABLE saved_jobs (
    user_id BIGINT REFERENCES users(id) ON DELETE CASCADE,
    job_id BIGINT REFERENCES jobs(id) ON DELETE CASCADE,
    saved_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (user_id, job_id)
);

CREATE TABLE company_followers (
    user_id BIGINT REFERENCES users(id) ON DELETE CASCADE,
    company_id BIGINT REFERENCES companies(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (user_id, company_id)
);
