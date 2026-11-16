# Opportunity-X

### Student Opportunity and Campus Engagement Portal

Opportunity-X is a web-based platform for Graphic Era Hill University students to discover campus opportunities, explore clubs, register for events, and receive important announcements.

---

## Project Objectives

- Provide a common platform for students to explore university clubs and events.
- Help students discover opportunities based on their skills and interests.
- Allow students to maintain their profiles and manage their skills.
- Enable administrators to manage students, clubs, events, notices, and registrations.
- Allow clubs to register, manage events, and publish notices after university approval.
- Keep students informed through targeted announcements and notifications.

---

## Features

### Student Module
- Student registration and login (backed by Spring Boot + SQL Server)
- Student profile management (name, email, course, year)
- Skills and interests selection (persisted to database for backend users)
- Club discovery and membership
- Event discovery and registration
- Notifications and announcements
- View club and event registrations
- Skill-based club and event recommendations

### University Admin Module
- Admin login (backed by Spring Boot + SQL Server)
- Dashboard overview (student, club, event, notice counts)
- Student management (view registered students)
- Club management (view and approve/reject club applications)
- Event management (create and view events)
- Notice publishing (targeted by skill or all students)
- Registration overview

### Club Admin Module
- Club registration application (submitted to university for approval)
- Club login (available once application is approved)
- Event management (create and view club events)
- Notice publishing for club members

---

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Backend | Java 21, Spring Boot 3.3.5 |
| Database | Microsoft SQL Server (JDBC Template) |
| Auth (students/clubs/admins) | PBKDF2WithHmacSHA256 / SHA-256 password hashing |
| Prototype data store | Browser localStorage (for pages not yet wired to the backend) |

---

## Project Structure

```text
ox/
|
|-- index.html                  # Public homepage
|-- student-login.html          # Student login (backend API)
|-- student-register.html       # Student registration (backend API)
|-- admin-login.html            # University admin login (backend API)
|-- admin-register.html         # Club registration form (submits to backend)
|-- club-login.html             # Club admin login (backend API)
|-- README.md
|
|-- assets/
|   |-- gehu-logo.png
|   |-- opportunity-x-logo.png
|
|-- css/
|   |-- style.css
|
|-- js/
|   |-- app.js                      # Shared frontend logic and localStorage portal
|   |-- student-login-api.js        # Student login → backend API
|   |-- student-register-api.js     # Student registration → backend API
|   |-- student-profile-api.js      # Student profile load/save → backend API
|
|-- admin/
|   |-- dashboard.html
|   |-- student.html
|   |-- clubs.html
|   |-- events.html
|   |-- notices.html
|   |-- registrations.html
|
|-- club-admin/
|   |-- dashboard.html
|   |-- events.html
|   |-- notices.html
|
|-- student/
|   |-- dashboard.html
|   |-- profile.html
|   |-- skills.html
|   |-- clubs.html
|   |-- events.html
|   |-- notifications.html
|   |-- registrations.html
|
|-- opportunityx-backend/           # Spring Boot backend
    |-- pom.xml
    |-- schema.sql                  # SQL Server schema — run once in SSMS
    |-- src/main/java/com/opportunityx/opportunityxbackend/
    |   |-- OpportunityxBackendApplication.java
    |   |-- StudentController.java
    |   |-- ClubController.java
    |   |-- AdminController.java
    |   |-- GlobalExceptionHandler.java
    |   |-- DatabaseTestController.java
    |   |-- TestController.java
    |   |-- model/
    |       |-- Student.java
    |       |-- StudentRegistrationRequest.java
    |-- src/main/resources/
        |-- application.properties
```

---

## How to Run

### Prerequisites
- Java 21
- Maven
- Microsoft SQL Server (local instance)
- Visual Studio Code with the Live Server extension (for the frontend)

### 1. Set up the database

1. Open SQL Server Management Studio (SSMS).
2. Connect to your local SQL Server instance.
3. Open `opportunityx-backend/schema.sql` and execute it (F5).
   - This creates the `OpportunityX` database and all tables with seed data.

### 2. Start the backend

```bash
cd opportunityx-backend
./mvnw spring-boot:run        # macOS / Linux
mvnw.cmd spring-boot:run      # Windows
```

The API starts at `http://localhost:8080`.

By default the backend uses **Windows Authentication** to connect to SQL Server.
To use SQL Server Authentication instead, edit `src/main/resources/application.properties`
and uncomment the OPTION B lines with your username and password.

### 3. Start the frontend

Open the `ox/` folder in VS Code and click **Go Live** (Live Server).
The app opens at `http://127.0.0.1:5500`.

### Default credentials

| Role | Username | Password |
|------|----------|----------|
| University Admin | `admin` | `12345678` |
| Seed Student 1 | `STU001` or `ananya@college.edu` | `student123` |
| Seed Student 2 | `STU002` or `aarav@college.edu` | `student123` |

> Club admin credentials are created when a club registration is submitted and approved.

---

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/admins/login` | University admin login |
| GET | `/api/students` | List all students |
| POST | `/api/students/register` | Register a new student |
| POST | `/api/students/login` | Student login |
| GET | `/api/students/{id}/profile` | Get student profile (auth required) |
| PUT | `/api/students/{id}/profile` | Update student profile (auth required) |
| PUT | `/api/students/{id}/skills` | Update student skills (auth required) |
| GET | `/api/clubs` | List approved clubs |
| POST | `/api/clubs/applications` | Submit a club registration application |
| GET | `/api/clubs/applications` | List all club applications |
| PUT | `/api/clubs/applications/{id}/status` | Approve or reject a club application |
| POST | `/api/clubs/login` | Club admin login |

---

## Architecture Notes

The project uses a **hybrid data model** during its transition from prototype to production:

- Student registration, login, profile, and skills are fully backed by SQL Server.
- Admin and club admin login are fully backed by SQL Server.
- Club registration applications and approvals are fully backed by SQL Server.
- Events, notices, club memberships, and event registrations are currently managed via **browser localStorage** and have not yet been migrated to the backend.

The `app.js` file bridges backend-authenticated sessions into localStorage so that localStorage-based portal pages continue to work for students who logged in via the API.

---

## Institution

Graphic Era Hill University, Dehradun, Uttarakhand
