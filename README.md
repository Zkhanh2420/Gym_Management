# Gym Management System

A comprehensive gym management system built with **Spring Boot 3** and **PostgreSQL**, supporting multiple user roles, online payment integration, and real-time communication via WebSocket.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture & Authorization](#architecture--authorization)
- [Installation & Setup](#installation--setup)
- [Environment Configuration](#environment-configuration)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)

---

## Features

### Admin / Staff
- Customer management: list, search, view detailed profiles
- Employee management: add/edit/delete employees (Staff & Trainer), categorized by department
- Service management: create, update, and delete service packages (by type and duration)
- Department management: full CRUD for gym departments
- Class & schedule management: create and organize training schedules
- Export reports to Excel (Apache POI)
- View login history and system logs

### Customer (User)
- Register / login with username & password or Google OAuth2
- Register for gym services and training packages
- Online payment via **VNPay** and **MoMo**
- View service registration history
- Use referral codes
- Real-time chat / support via WebSocket

### Trainer
- View assigned class list
- Manage teaching schedule

### General
- Secure authentication with **JWT** + **Spring Security**
- Quick login via **Google OAuth2**
- Real-time chat via **STOMP over WebSocket**
- Checkpoint / attendance tracking
- Search autocomplete

---

## Tech Stack

| Component | Technology |
|---|---|
| Backend | Java 21, Spring Boot 3.4.5 |
| Database | PostgreSQL (Oracle scripts also available) |
| ORM | Spring Data JPA / Hibernate |
| Security | Spring Security, JWT (auth0 java-jwt), Google OAuth2 |
| Real-time | Spring WebSocket, STOMP, Spring Security Messaging |
| Payment | VNPay, MoMo |
| Template Engine | Thymeleaf |
| Export | Apache POI (Excel) |
| API Docs | SpringDoc OpenAPI (Swagger UI) |
| Build Tool | Maven |
| Container | Docker |
| Utilities | Lombok, dotenv-java |

---

## Architecture & Authorization

The system uses **Role-Based Access Control (RBAC)** with 4 main roles:

```
ADMIN    Full system administration
STAFF    Manage customers, services, and employees
TRAINER  View teaching schedule, manage assigned classes
USER     Register for services, make payments
```

---

## Installation & Setup

### Prerequisites

- Java 21+
- Maven 3.8+
- PostgreSQL 14+
- (Optional) Docker

### Step 1: Clone the repository

```bash
git clone <repository-url>
cd QLyPhongGym-main
```

### Step 2: Initialize the database

Run the SQL scripts in order from the `PostgreDatabase/` directory:

```bash
# 1. Create schema and tables
psql -U <user> -d <database> -f PostgreDatabase/QLyPhongGym.sql

# 2. Create stored procedures and functions
psql -U <user> -d <database> -f PostgreDatabase/Dich_Vu_Procedure.sql
psql -U <user> -d <database> -f PostgreDatabase/DangKyDichVu_Procedure_PosgreVer.sql

# 3. Load sample data
psql -U <user> -d <database> -f PostgreDatabase/SampleData.sql
```

### Step 3: Configure environment variables

Create a `.env` file inside the `Java/` directory (see [Environment Configuration](#environment-configuration)).

### Step 4: Run the application

```bash
cd Java
./mvnw spring-boot:run
```

The application will be available at: `http://localhost:8081`

### Run with Docker

```bash
cd Java
docker build -t qlyphonggym .
docker run -p 8081:8081 --env-file .env qlyphonggym
```

---

## Environment Configuration

Create a `Java/.env` file with the following content:

```env
# Database
DB_URL=jdbc:postgresql://localhost:5432/<database_name>
DB_USERNAME=<pg_username>
DB_PASSWORD=<pg_password>

# JWT
JWT_SECRET_KEY=<your_jwt_secret>

# Google OAuth2
GOOGLE_CLIENT_ID=<google_client_id>
GOOGLE_CLIENT_SECRET=<google_client_secret>

# VNPay
VNPAY_TMN_CODE=<vnpay_tmn_code>
VNPAY_HASH_SECRET=<vnpay_hash_secret>
VNPAY_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html

# MoMo
MOMO_PARTNER_CODE=<momo_partner_code>
MOMO_ACCESS_KEY=<momo_access_key>
MOMO_SECRET_KEY=<momo_secret_key>
```

---

## Project Structure

```
QLyPhongGym-main/
 Java/                          # Spring Boot source code
    src/main/java/hahaha/
       config/                # Security, VNPay, WebSocket configuration
       controller/            # REST Controllers & MVC Controllers
       DTO/                   # Data Transfer Objects
       enums/                 # Enums: EmployeeType, ServiceType, ...
       mapper/                # Entity-DTO mappers
       model/                 # JPA Entities
       repository/            # Spring Data JPA Repositories
       service/               # Business Logic
       validation/            # Custom validators
    src/main/resources/
       templates/             # Thymeleaf HTML templates
          Admin/
          Staff/
          Trainer/
          User/
       static/                # CSS, JS, Images
    pom.xml
    Dockerfile
 PostgreDatabase/               # PostgreSQL scripts
 OracleDatabase/                # Oracle scripts (optional)
 README.md
```

---

## API Documentation

Once the application is running, access the Swagger UI at:

```
http://localhost:8081/swagger-ui/index.html
```

Main API groups:

| Endpoint | Description |
|---|---|
| `/api/khach-hang` | Customer management |
| `/api/nhan-vien` | Employee management |
| `/api/quan-ly-dich-vu` | Service management |
| `/api/quan-ly-bo-mon` | Department management |
| `/api/quan-ly-lop` | Class management |
| `/api/lich-lop` | Class schedule |
| `/api/dang-ky-dich-vu` | Service registration |
| `/api/vnpay` | VNPay payment |
| `/api/momo` | MoMo payment |
| `/api/checkpoint` | Attendance / checkpoint |
| `/ws` | WebSocket endpoint (STOMP) |
