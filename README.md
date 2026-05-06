# Secure Employee Management REST API

A production-style RESTful API built with **Spring Boot** for managing employees, featuring full CRUD operations, role-based access control, and BCrypt password encryption.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot |
| Security | Spring Security + BCrypt |
| Database | MySQL |
| ORM | JPA / Hibernate |
| Auth Storage | JDBC (custom tables) |
| Build Tool | Maven |
| Testing | Postman |

---

## Features

- Full **CRUD** operations — `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
- **3-layer architecture** — Controller → Service → Repository
- **Role-based access control** with three roles:
  - `EMPLOYEE` — read access
  - `MANAGER` — read + write
  - `ADMIN` — full access including delete
- **BCrypt password encryption** for all stored credentials
- **JDBC-based authentication** against a real database with custom queries
- **Global exception handling** via `@ControllerAdvice` with structured JSON error responses
- **Partial updates** via `PATCH` with dynamic field handling
- Sensitive config managed via **environment variables** — no hardcoded credentials

---

## Project Structure

```
src/
├── main/
│   ├── java/com/luv2code/springboot/cruddemo/
│   │   ├── dao/               # Data access layer (JpaRepository)
│   │   ├── entity/            # JPA entity (Employee)
│   │   ├── exception/         # Custom exceptions + global handler
│   │   ├── rest/              # REST controller
│   │   ├── security/          # Spring Security config
│   │   └── service/           # Business logic
│   └── resources/
│       └── application.properties
└── sql-scripts/               # DB setup scripts
```

---

## Getting Started

### Prerequisites

- Java 17+
- MySQL
- Maven

### 1. Clone the repo

```bash
git clone https://github.com/OmarIbrahimF/employee-rest-api.git
cd employee-rest-api
```

### 2. Set up the database

Run the SQL scripts in order:

```bash
mysql -u root -p < sql-scripts/employee-directory.sql
mysql -u root -p < sql-scripts/05-setup-spring-security-demo-database-bcrypt.sql
```

### 3. Configure environment variables

```bash
export DB_USERNAME=your_db_username
export DB_PASSWORD=your_db_password
```

### 4. Run the app

```bash
mvn spring-boot:run
```

The API will be available at `http://localhost:8080`

---

## API Endpoints

| Method | Endpoint | Role Required | Description |
|---|---|---|---|
| `GET` | `/api/employees` | EMPLOYEE | Get all employees |
| `GET` | `/api/employees/{id}` | EMPLOYEE | Get employee by ID |
| `POST` | `/api/employees` | MANAGER | Add new employee |
| `PUT` | `/api/employees` | MANAGER | Full update |
| `PATCH` | `/api/employees/{id}` | MANAGER | Partial update |
| `DELETE` | `/api/employees/{id}` | ADMIN | Delete employee |

### Example Response — Get Employee

```json
{
  "id": 1,
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@company.com"
}
```

### Example Response — Employee Not Found

```json
{
  "status": 404,
  "message": "Employee not found with id - 99",
  "timeStamp": 1714123456789
}
```

---

## Security

Authentication is HTTP Basic. Passwords are stored as **BCrypt hashes** in the database — never in plain text.

```
EMPLOYEE role  → GET only
MANAGER role   → GET, POST, PUT, PATCH
ADMIN role     → all of the above + DELETE
```

---

## Author

**Omar Ibrahim**
[LinkedIn](https://linkedin.com/in/omarihassaan) · [GitHub](https://github.com/OmarIbrahimF)
