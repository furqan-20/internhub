# InternHub Requirements Document

## Project Name
InternHub

## Project Type
Full-Stack Web Application

## Tech Stack
- Backend: Django
- API: Django REST Framework (DRF)
- Frontend: React
- Database: PostgreSQL
- Authentication: JWT

---

# Problem Statement

Students apply to internships through multiple platforms such as LinkedIn, company websites, university notices, emails, and job portals. As the number of applications increases, students often lose track of:

- Application status
- Deadlines
- Resume versions
- Company information
- Interview schedules

InternHub provides a centralized platform that helps students manage internship opportunities and track their applications efficiently.

---

# Project Vision

To create a modern internship tracking platform that allows students to discover opportunities, manage applications, and monitor their progress throughout the internship recruitment process.

---

# Target Users

## Student
A user who wants to browse internships and track applications.

## Recruiter/Admin
A user who posts internship opportunities and manages applicants.

## Super Admin
A user responsible for managing the entire platform.

---

# User Roles

## Student Permissions

- Register account
- Login/logout
- Create profile
- Update profile
- Upload resume
- Browse internships
- Search internships
- Filter internships
- Apply to internships
- Track application status
- Save internships

## Recruiter/Admin Permissions

- Login
- Create internship posts
- Update internship posts
- Delete internship posts
- View applicants
- Update application status

## Super Admin Permissions

- Manage all users
- Manage all internships
- Access Django admin panel

---

# MVP Scope

The first version of InternHub should include:

## Authentication

- User registration
- User login
- JWT authentication
- Protected API endpoints

## Student Profile

- Full name
- University
- Degree program
- Graduation year
- Skills
- Resume upload

## Internship Management

- Internship title
- Company name
- Location
- Internship type
- Description
- Requirements
- Application deadline

## Application Tracking

- Apply to internship
- View application history
- Track application status

Statuses:

- Applied
- Shortlisted
- Interview
- Selected
- Rejected
- Withdrawn

## Search and Filtering

- Search by internship title
- Search by company name
- Filter by location
- Filter by internship type
- Sort by deadline
- Sort by newest

## Dashboard

- Total applications
- Applications by status
- Upcoming deadlines
- Recent applications

---

# Functional Requirements

## Authentication

### FR-01
The system shall allow users to register.

### FR-02
The system shall allow users to login using email and password.

### FR-03
The system shall provide JWT authentication.

### FR-04
The system shall restrict protected endpoints to authenticated users.

---

## Profile Management

### FR-05
Students shall be able to create a profile.

### FR-06
Students shall be able to update their profile.

### FR-07
Students shall be able to upload a resume.

### FR-08
Students shall be able to view their profile.

---

## Internship Management

### FR-09
Admins shall be able to create internship posts.

### FR-10
Admins shall be able to edit internship posts.

### FR-11
Admins shall be able to delete internship posts.

### FR-12
Students shall be able to view internships.

### FR-13
Students shall be able to view internship details.

### FR-14
The system shall display internship deadlines.

---

## Application Management

### FR-15
Students shall be able to apply to internships.

### FR-16
Students shall not be able to apply twice to the same internship.

### FR-17
Students shall be able to view their applications.

### FR-18
Admins shall be able to view applicants.

### FR-19
Admins shall be able to update application statuses.

### FR-20
Students shall be able to withdraw applications.

---

## Search and Filtering

### FR-21
Students shall be able to search internships by title.

### FR-22
Students shall be able to search internships by company.

### FR-23
Students shall be able to filter internships by location.

### FR-24
Students shall be able to filter internships by internship type.

### FR-25
Students shall be able to sort internships by deadline.

### FR-26
Students shall be able to sort internships by newest.

---

## Dashboard

### FR-27
The system shall show total applications.

### FR-28
The system shall show application counts grouped by status.

### FR-29
The system shall show upcoming deadlines.

### FR-30
The system shall show recent applications.

---

# Non-Functional Requirements

## Security

### NFR-01
Passwords shall be securely hashed.

### NFR-02
Protected APIs shall require authentication.

### NFR-03
Users shall not access private data belonging to other users.

### NFR-04
Recruiters shall only manage internships they created.

### NFR-05
Uploaded files shall be validated.

---

## Performance

### NFR-06
Internship listings shall load efficiently.

### NFR-07
Search and filter operations shall return results quickly.

### NFR-08
Dashboard queries shall be optimized.

---

## Usability

### NFR-09
The interface shall be easy to use.

### NFR-10
Validation errors shall be clearly displayed.

### NFR-11
The application shall be responsive.

---

## Maintainability

### NFR-12
Backend code shall be organized into separate Django apps.

### NFR-13
APIs shall follow REST principles.

### NFR-14
The project shall contain documentation.

### NFR-15
The project shall contain API tests.

---

# Business Rules

1. A student may apply to an internship only once.
2. Only authenticated users may apply.
3. Only recruiters/admins may create internships.
4. Recruiters may edit only their own internships.
5. Students may only view their own applications.
6. Expired internships shall not accept applications.
7. Application statuses may only be updated by recruiters/admins.
8. Resume uploads must be valid files.
9. Every internship must have a deadline.
10. Every internship must contain a title and description.

---

# User Stories

## Student

- As a student, I want to register an account so that I can access the platform.
- As a student, I want to create my profile so that recruiters can view my information.
- As a student, I want to upload my resume so that I can apply easily.
- As a student, I want to browse internships so that I can discover opportunities.
- As a student, I want to track my application status so that I know my progress.

## Recruiter/Admin

- As a recruiter, I want to create internship posts so that students can apply.
- As a recruiter, I want to manage applicants so that I can review applications.
- As a recruiter, I want to update statuses so that students can track their progress.

---

# Future Enhancements

- Email notifications
- Deadline reminders
- Resume version management
- AI resume review
- Internship recommendation system
- Calendar integration
- Docker support
- CI/CD pipeline
- University career center integration

---

# Project Status

Current Phase: Requirements Engineering

Next Phase: System Design
