# InternHub Database Design

## 1. Database Overview

InternHub will use a relational database.

For development, we may start with SQLite.

For the final professional version, we will use PostgreSQL.

The main database tables will be:

```text
CustomUser
Profile
Internship
Application
SavedInternship
```

---

## 2. Important Design Decision

We will use a custom user model from the beginning.

Reason:

Django has a default User model, but in professional projects, if we need custom fields like `role`, it is better to create a custom user model early.

Changing the user model later is difficult.

So we will create:

```text
CustomUser
```

inside the `accounts` app.

---

## 3. Entity Relationship Diagram

```text
CustomUser
    |
    | one-to-one
    v
Profile


CustomUser
    |
    | one-to-many
    v
Internship


CustomUser
    |
    | one-to-many
    v
Application
    ^
    |
Internship


CustomUser
    |
    | one-to-many
    v
SavedInternship
    ^
    |
Internship
```

---

## 4. Table: CustomUser

The `CustomUser` table stores authentication and role information.

### Purpose

To manage users who can be students, recruiters, or admins.

### Fields

| Field | Type | Description |
|---|---|---|
| id | AutoField/BigAutoField | Unique user ID |
| username | CharField | Username |
| email | EmailField | User email |
| password | CharField | Hashed password |
| role | CharField | User role |
| is_active | BooleanField | Shows if user account is active |
| is_staff | BooleanField | Allows access to admin panel |
| is_superuser | BooleanField | Full admin permission |
| date_joined | DateTimeField | Account creation date |

### Role Choices

```text
student
recruiter
admin
```

### Rules

1. Email should be unique.
2. Every user must have one role.
3. Password must be stored in hashed form.
4. Super admin is managed by Django admin system.

---

## 5. Table: Profile

The `Profile` table stores extra user information.

### Purpose

To store student or recruiter profile details.

### Fields

| Field | Type | Description |
|---|---|---|
| id | BigAutoField | Unique profile ID |
| user | OneToOneField | Connected user |
| full_name | CharField | Full name |
| university | CharField | University name |
| degree_program | CharField | Degree program |
| graduation_year | PositiveIntegerField | Graduation year |
| skills | TextField | Student skills |
| resume | FileField | Uploaded resume |
| created_at | DateTimeField | Profile creation date |
| updated_at | DateTimeField | Last update date |

### Relationship

```text
One CustomUser has one Profile.
```

Example:

```text
CustomUser 1 ---- Profile 1
```

### Rules

1. A user can have only one profile.
2. A profile belongs to only one user.
3. Resume should be a PDF file.
4. Resume file size should be limited.

---

## 6. Table: Internship

The `Internship` table stores internship posts.

### Purpose

To allow recruiters/admins to create internship opportunities.

### Fields

| Field | Type | Description |
|---|---|---|
| id | BigAutoField | Unique internship ID |
| title | CharField | Internship title |
| company_name | CharField | Company name |
| location | CharField | Internship location |
| internship_type | CharField | Type of internship |
| description | TextField | Internship description |
| requirements | TextField | Required skills or qualifications |
| deadline | DateField | Last date to apply |
| created_by | ForeignKey | Recruiter/admin who created the post |
| is_active | BooleanField | Shows if internship is active |
| created_at | DateTimeField | Created date |
| updated_at | DateTimeField | Last update date |

### Internship Type Choices

```text
remote
onsite
hybrid
```

### Relationship

```text
One recruiter/admin can create many internships.
```

Example:

```text
CustomUser 1 ---- Internship 1
CustomUser 1 ---- Internship 2
CustomUser 1 ---- Internship 3
```

### Rules

1. Only recruiters/admins can create internships.
2. A recruiter can only update or delete internships they created.
3. Deadline cannot be in the past.
4. Expired internships should not accept new applications.
5. Each internship must have title, company name, description, and deadline.

---

## 7. Table: Application

The `Application` table stores internship applications submitted by students.

### Purpose

To track which student applied to which internship and what their current status is.

### Fields

| Field | Type | Description |
|---|---|---|
| id | BigAutoField | Unique application ID |
| student | ForeignKey | Student who applied |
| internship | ForeignKey | Internship applied to |
| status | CharField | Current application status |
| applied_at | DateTimeField | Application submission date |
| updated_at | DateTimeField | Last status update |

### Status Choices

```text
applied
shortlisted
interview
selected
rejected
withdrawn
```

### Relationship

```text
One student can have many applications.
One internship can have many applications.
```

Example:

```text
Student 1 ---- Application 1 ---- Internship 1
Student 1 ---- Application 2 ---- Internship 2
```

### Rules

1. Only students can apply to internships.
2. A student cannot apply to the same internship twice.
3. A student cannot apply after the internship deadline.
4. Recruiter/admin can update the application status.
5. Student can withdraw their own application.
6. Student can only view their own applications.
7. Recruiter can only view applications for internships they created.

### Unique Constraint

```text
student + internship must be unique
```

Meaning:

```text
One student cannot apply to the same internship twice.
```

---

## 8. Table: SavedInternship

The `SavedInternship` table stores bookmarked internships.

### Purpose

To allow students to save internships and view them later.

### Fields

| Field | Type | Description |
|---|---|---|
| id | BigAutoField | Unique saved internship ID |
| student | ForeignKey | Student who saved the internship |
| internship | ForeignKey | Saved internship |
| saved_at | DateTimeField | Saved date |

### Relationship

```text
One student can save many internships.
One internship can be saved by many students.
```

Example:

```text
Student 1 ---- SavedInternship 1 ---- Internship 1
Student 1 ---- SavedInternship 2 ---- Internship 2
```

### Rules

1. Only students can save internships.
2. A student cannot save the same internship twice.
3. Student can remove internship from saved list.

### Unique Constraint

```text
student + internship must be unique
```

Meaning:

```text
One student cannot save the same internship twice.
```

---

## 9. Relationship Summary

| Relationship | Type |
|---|---|
| CustomUser to Profile | One-to-One |
| CustomUser to Internship | One-to-Many |
| CustomUser to Application | One-to-Many |
| Internship to Application | One-to-Many |
| CustomUser to SavedInternship | One-to-Many |
| Internship to SavedInternship | One-to-Many |

---

## 10. Database Constraints

### User Constraints

```text
email must be unique
role must be valid
```

### Profile Constraints

```text
user must be unique
resume must be PDF
```

### Internship Constraints

```text
title is required
company_name is required
description is required
deadline is required
deadline cannot be in the past
```

### Application Constraints

```text
student and internship combination must be unique
status must be valid
```

### SavedInternship Constraints

```text
student and internship combination must be unique
```

---

## 11. Indexing Plan

Indexes help the database search faster.

We may add indexes on commonly searched fields.

### Internship Indexes

```text
title
company_name
location
internship_type
deadline
```

### Application Indexes

```text
student
internship
status
```

### SavedInternship Indexes

```text
student
internship
```

---

## 12. Why We Are Not Creating a Company Table Yet

For the MVP, we will store company information directly inside the Internship table.

Example:

```text
company_name = "Systems Limited"
```

This keeps the first version simple.

Later, we can create a separate `Company` table with:

```text
company_name
company_logo
company_website
company_description
industry
```

That will be a future enhancement.

---

## 13. Future Database Improvements

Future tables may include:

```text
Company
ResumeVersion
Notification
InterviewSchedule
ApplicationNote
Skill
```

These are not part of the MVP.

---

## 14. Final Database Summary

The database design for InternHub will support:

1. User authentication
2. User roles
3. Student profiles
4. Resume uploads
5. Internship posts
6. Internship applications
7. Application status tracking
8. Saved internships
9. Role-based access
10. Duplicate prevention using constraints

This design is enough for the MVP and can be extended later.
