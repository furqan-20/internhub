# InternHub System Design

## 1. System Overview

InternHub is a full-stack web application that helps students manage internship opportunities and track their applications.

The system has two main parts:

1. Backend API built with Django and Django REST Framework
2. Frontend client built with React

The frontend communicates with the backend using REST APIs.

---

## 2. High-Level Architecture

```text
User Browser
    |
    v
React Frontend
    |
    | HTTP Requests
    | GET, POST, PUT, PATCH, DELETE
    v
Django REST Framework API
    |
    v
Django Models / Business Logic
    |
    v
PostgreSQL Database