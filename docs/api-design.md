# InternHub API Design

## 1. API Overview

InternHub will use REST APIs built with Django REST Framework.

The frontend will be built with React and will communicate with the backend through these APIs.

Base API URL:

```text
/api/
```

---

# 2. Authentication APIs

Authentication APIs handle registration, login, token refresh, and current user details.

---

## 2.1 Register User

### Endpoint

```http
POST /api/auth/register/
```

### Access

Public

### Description

Creates a new user account.

### Request Body

```json
{
  "username": "furqan",
  "email": "furqan@example.com",
  "password": "password123",
  "password2": "password123",
  "role": "student"
}
```

### Success Response

```json
{
  "message": "User registered successfully.",
  "user": {
    "id": 1,
    "username": "furqan",
    "email": "furqan@example.com",
    "role": "student"
  }
}
```

### Rules

- Email must be unique.
- Password and password2 must match.
- Role must be valid.
- Allowed roles: student, recruiter.

---

## 2.2 Login User

### Endpoint

```http
POST /api/auth/login/
```

### Access

Public

### Description

Logs in a user and returns JWT tokens.

### Request Body

```json
{
  "email": "furqan@example.com",
  "password": "password123"
}
```

### Success Response

```json
{
  "access": "access_token_here",
  "refresh": "refresh_token_here",
  "user": {
    "id": 1,
    "username": "furqan",
    "email": "furqan@example.com",
    "role": "student"
  }
}
```

---

## 2.3 Refresh Token

### Endpoint

```http
POST /api/auth/token/refresh/
```

### Access

Public

### Description

Generates a new access token using the refresh token.

### Request Body

```json
{
  "refresh": "refresh_token_here"
}
```

### Success Response

```json
{
  "access": "new_access_token_here"
}
```

---

## 2.4 Current User

### Endpoint

```http
GET /api/auth/me/
```

### Access

Authenticated user

### Description

Returns the currently logged-in user.

### Headers

```http
Authorization: Bearer access_token_here
```

### Success Response

```json
{
  "id": 1,
  "username": "furqan",
  "email": "furqan@example.com",
  "role": "student"
}
```

---

# 3. Profile APIs

Profile APIs handle student/recruiter profile data.

---

## 3.1 View Profile

### Endpoint

```http
GET /api/profile/
```

### Access

Authenticated user

### Description

Returns the logged-in user's profile.

### Success Response

```json
{
  "id": 1,
  "full_name": "Furqanullah",
  "university": "NED University of Engineering and Technology",
  "degree_program": "Software Engineering",
  "graduation_year": 2027,
  "skills": "Python, Django, React",
  "resume": "/media/resumes/resume.pdf"
}
```

---

## 3.2 Update Profile

### Endpoint

```http
PATCH /api/profile/
```

### Access

Authenticated user

### Description

Updates the logged-in user's profile.

### Request Body

```json
{
  "full_name": "Furqanullah",
  "university": "NED University of Engineering and Technology",
  "degree_program": "Software Engineering",
  "graduation_year": 2027,
  "skills": "Python, Django, Django REST Framework, React"
}
```

### Success Response

```json
{
  "message": "Profile updated successfully."
}
```

---

## 3.3 Upload Resume

### Endpoint

```http
PATCH /api/profile/resume/
```

### Access

Authenticated student

### Description

Uploads or updates the student's resume.

### Request Type

```text
multipart/form-data
```

### Request Body

```text
resume: resume.pdf
```

### Rules

- File must be PDF.
- File size should be limited.
- Only authenticated users can upload.

---

# 4. Internship APIs

Internship APIs handle internship posting, listing, searching, filtering, updating, and deleting.

---

## 4.1 List Internships

### Endpoint

```http
GET /api/internships/
```

### Access

Public or authenticated user

### Description

Returns all active internships.

### Query Parameters

```text
search=django
location=Karachi
internship_type=remote
ordering=deadline
```

### Example

```http
GET /api/internships/?search=django&location=Karachi
```

### Success Response

```json
[
  {
    "id": 1,
    "title": "Backend Developer Intern",
    "company_name": "TechSoft",
    "location": "Karachi",
    "internship_type": "onsite",
    "deadline": "2026-07-20",
    "is_active": true
  }
]
```

---

## 4.2 Create Internship

### Endpoint

```http
POST /api/internships/
```

### Access

Recruiter/Admin only

### Description

Creates a new internship post.

### Request Body

```json
{
  "title": "Backend Developer Intern",
  "company_name": "TechSoft",
  "location": "Karachi",
  "internship_type": "onsite",
  "description": "Work with Django APIs.",
  "requirements": "Python, Django, REST APIs",
  "deadline": "2026-07-20"
}
```

### Success Response

```json
{
  "message": "Internship created successfully.",
  "internship": {
    "id": 1,
    "title": "Backend Developer Intern"
  }
}
```

### Rules

- Only recruiter/admin can create internships.
- Deadline cannot be in the past.
- Title, company name, description, and deadline are required.

---

## 4.3 Internship Detail

### Endpoint

```http
GET /api/internships/{id}/
```

### Access

Public or authenticated user

### Description

Returns details of one internship.

### Success Response

```json
{
  "id": 1,
  "title": "Backend Developer Intern",
  "company_name": "TechSoft",
  "location": "Karachi",
  "internship_type": "onsite",
  "description": "Work with Django APIs.",
  "requirements": "Python, Django, REST APIs",
  "deadline": "2026-07-20",
  "created_by": "recruiter@example.com",
  "is_active": true
}
```

---

## 4.4 Update Internship

### Endpoint

```http
PATCH /api/internships/{id}/
```

### Access

Internship owner or admin only

### Description

Updates an internship post.

### Request Body

```json
{
  "location": "Remote",
  "internship_type": "remote"
}
```

### Rules

- Only the internship creator can update it.
- Super admin can update any internship.

---

## 4.5 Delete Internship

### Endpoint

```http
DELETE /api/internships/{id}/
```

### Access

Internship owner or admin only

### Description

Deletes an internship post.

### Success Response

```json
{
  "message": "Internship deleted successfully."
}
```

---

# 5. Application APIs

Application APIs handle applying, viewing applications, withdrawing, and updating application status.

---

## 5.1 Apply to Internship

### Endpoint

```http
POST /api/internships/{id}/apply/
```

### Access

Student only

### Description

Allows a student to apply to an internship.

### Success Response

```json
{
  "message": "Application submitted successfully.",
  "application": {
    "id": 1,
    "internship": 1,
    "status": "applied"
  }
}
```

### Rules

- Only students can apply.
- Student cannot apply twice to the same internship.
- Student cannot apply after deadline.
- Student must be authenticated.

---

## 5.2 My Applications

### Endpoint

```http
GET /api/applications/
```

### Access

Authenticated student

### Description

Returns all applications submitted by the logged-in student.

### Success Response

```json
[
  {
    "id": 1,
    "internship": {
      "id": 1,
      "title": "Backend Developer Intern",
      "company_name": "TechSoft"
    },
    "status": "applied",
    "applied_at": "2026-06-26T10:30:00Z"
  }
]
```

---

## 5.3 Application Detail

### Endpoint

```http
GET /api/applications/{id}/
```

### Access

Application owner or internship recruiter/admin

### Description

Returns detail of a single application.

---

## 5.4 Withdraw Application

### Endpoint

```http
PATCH /api/applications/{id}/withdraw/
```

### Access

Application owner only

### Description

Allows a student to withdraw their application.

### Success Response

```json
{
  "message": "Application withdrawn successfully.",
  "status": "withdrawn"
}
```

---

## 5.5 Update Application Status

### Endpoint

```http
PATCH /api/applications/{id}/status/
```

### Access

Internship owner or admin only

### Description

Allows recruiter/admin to update application status.

### Request Body

```json
{
  "status": "shortlisted"
}
```

### Allowed Statuses

```text
applied
shortlisted
interview
selected
rejected
withdrawn
```

### Success Response

```json
{
  "message": "Application status updated successfully.",
  "status": "shortlisted"
}
```

---

## 5.6 View Applicants for Internship

### Endpoint

```http
GET /api/internships/{id}/applicants/
```

### Access

Internship owner or admin only

### Description

Returns all students who applied to a specific internship.

### Success Response

```json
[
  {
    "application_id": 1,
    "student": {
      "id": 2,
      "full_name": "Furqanullah",
      "university": "NED University of Engineering and Technology",
      "degree_program": "Software Engineering"
    },
    "status": "applied",
    "applied_at": "2026-06-26T10:30:00Z"
  }
]
```

---

# 6. Saved Internship APIs

Saved internship APIs handle bookmarking internships.

---

## 6.1 Save Internship

### Endpoint

```http
POST /api/internships/{id}/save/
```

### Access

Student only

### Description

Saves/bookmarks an internship.

### Success Response

```json
{
  "message": "Internship saved successfully."
}
```

### Rules

- Student cannot save the same internship twice.

---

## 6.2 Unsave Internship

### Endpoint

```http
DELETE /api/internships/{id}/unsave/
```

### Access

Student only

### Description

Removes internship from saved list.

### Success Response

```json
{
  "message": "Internship removed from saved list."
}
```

---

## 6.3 My Saved Internships

### Endpoint

```http
GET /api/saved-internships/
```

### Access

Authenticated student

### Description

Returns all internships saved by the logged-in student.

---

# 7. Dashboard APIs

Dashboard APIs return summary statistics.

---

## 7.1 Student Dashboard

### Endpoint

```http
GET /api/dashboard/student/
```

### Access

Student only

### Description

Returns student dashboard data.

### Success Response

```json
{
  "total_applications": 10,
  "applied": 4,
  "shortlisted": 2,
  "interview": 1,
  "selected": 1,
  "rejected": 2,
  "upcoming_deadlines": [
    {
      "id": 3,
      "title": "Frontend Intern",
      "deadline": "2026-07-01"
    }
  ]
}
```

---

## 7.2 Recruiter Dashboard

### Endpoint

```http
GET /api/dashboard/recruiter/
```

### Access

Recruiter/Admin only

### Description

Returns recruiter dashboard data.

### Success Response

```json
{
  "total_internships_posted": 5,
  "active_internships": 4,
  "total_applicants": 25,
  "recent_applications": [
    {
      "application_id": 1,
      "student_name": "Furqanullah",
      "internship_title": "Backend Developer Intern",
      "status": "applied"
    }
  ]
}
```

---

# 8. HTTP Methods Used

| Method | Purpose |
|---|---|
| GET | Read data |
| POST | Create data |
| PUT | Replace full data |
| PATCH | Update partial data |
| DELETE | Delete data |

---

# 9. Permission Summary

| API Group | Student | Recruiter/Admin | Public |
|---|---|---|---|
| Register | Yes | Yes | Yes |
| Login | Yes | Yes | Yes |
| View internships | Yes | Yes | Yes |
| Create internship | No | Yes | No |
| Apply to internship | Yes | No | No |
| View own applications | Yes | No | No |
| View applicants | No | Yes | No |
| Update application status | No | Yes | No |
| Save internships | Yes | No | No |
| Dashboard | Yes | Yes | No |

---

# 10. Error Response Format

All error responses should follow a clear format.

```json
{
  "success": false,
  "message": "Something went wrong.",
  "errors": {
    "field_name": ["Error message here."]
  }
}
```

Example:

```json
{
  "success": false,
  "message": "You have already applied to this internship.",
  "errors": {
    "internship": ["Duplicate application is not allowed."]
  }
}
```

---

# 11. Success Response Format

Common success response:

```json
{
  "success": true,
  "message": "Operation completed successfully.",
  "data": {}
}
```

---

# 12. API Design Summary

InternHub APIs will support:

1. User registration and login
2. JWT authentication
3. Profile management
4. Resume upload
5. Internship posting
6. Internship search and filtering
7. Internship applications
8. Application status tracking
9. Saved internships
10. Student and recruiter dashboards

This API design will guide the backend implementation in Django REST Framework.
