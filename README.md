<<<<<<< HEAD
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
=======
# ALX ProDev Backend Web Development

# ProDev Backend Engineering Program Overview
<p>The program was focused on Advanced Backend. Therefore most of the tasks and concepts required an prior understanding of Backend Web Development.</p>
<p>The program divided deeper into deployment, testing, containerization among other concepts that are required to bring the web application to life and accessible to the outside world.</p>
<p>The program also help us understand the benefits of creating monthly worklogs and importance of peer collaboration. </p>

# Key technologies covered:
1. Advanced SQL: for making advanced queries, indexing and optimazation.
2. Python: we solidified our understanding on generators, decorators, context managers and asynchronous programming.
3. Django: we enhanced our understanding of the framework with some advanced concepts such as middlewares and signals.
4. Shell: we learnt the basics and advanced concepts of shell.
5. Docker: we learnt and implemented the basics of docker.
6. Kubernetes: Kubernetes (K8s) is an open-source platform for automating the deployment, scaling, and management of containerized applications. It provides the orchestration needed to manage containerized apps at scale.
7. GraphQL: GraphQL is a powerful query language and runtime for APIs, developed by Facebook, that allows clients to request exactly the data they need — nothing more, nothing less. Unlike REST APIs, which return fixed data structures, GraphQL gives clients the flexibility to shape the response format.
8. CI/CD pipelines: This concept page introduced CI/CD principles and walks through setting up pipelines using tools like Jenkins, Travis CI, and GitHub Actions, managing environments, and incorporating feedback loops.

# Important Backend Development concepts:
1. Security.
2. Deployment.
3. Automation.
4. Database design.
5. Advanced Django features.
5. Shell basics.
6. API endpoints.
7. Asynchronous programming.
8. Testing.
9. Caching strategies.

# Challenges faced:
* Working with docker.
* Keeping up with my monthly worklogs.
* Implementing some concepts learnt.

# Solutions Implemented:
* Updating monthly worklogs to fit my schedules.
* Doing more research and watching tutorials on the challenging concepts.
* Understanding docker from scratch and doing minimal tasks with it to get a grasp of its functionality.

# Best practices and personal takeaways:
1. Ask for help when stuck.
2. Be open to feedback.
3. Keep challenging myself to make it.

# API endpoints:
>>>>>>> 7c75d81ac9ca8c44c168a1a1b2fe8a55d0ec7295
