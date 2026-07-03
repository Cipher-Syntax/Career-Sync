# CareerSync Build Plan

This document breaks the entire CareerSync project down into specific, scoped, and verifiable units. Each unit produces one visible result and stays within a single system boundary.

## Phase 1: Foundation & Authentication

### Unit 01: Django Backend Initialization & Core User Models
- **Boundary:** Django Backend
- **Builds:** Django project initialization, base configuration, custom User model, and PostgreSQL connection setup (no `pgvector` yet).
- **Dependencies:** None.
- **Done when:** The Django development server runs, connects to Postgres, and the admin panel is accessible.

### Unit 02: React Frontend Initialization & Routing
- **Boundary:** React Frontend
- **Builds:** Vite/React project setup, TailwindCSS + shadcn/ui configuration, and React Router with empty placeholder pages (Login, Dashboard, Settings).
- **Dependencies:** None.
- **Done when:** The React app compiles and navigating to `/dashboard` shows the empty placeholder.

### Unit 03: JWT Authentication & OTP Registration API
- **Boundary:** Django Backend
- **Builds:** Integration of `dj-rest-auth`, endpoints for Email OTP registration, and JWT token generation.
- **Dependencies:** Unit 01.
- **Done when:** Postman/cURL can successfully register a user via OTP and retrieve a valid JWT.

### Unit 04: Frontend Auth Flow & Protected Routes
- **Boundary:** React Frontend
- **Builds:** Login/Signup forms, OTP entry UI, JWT local storage logic, and protected route wrapper enforcing auth on the Dashboard.
- **Dependencies:** Unit 02, Unit 03.
- **Done when:** A user can register, verify OTP, log in through the UI, and get redirected to the protected dashboard.

## Phase 2: The AI Microservice

### Unit 05: FastAPI Microservice Initialization & Document Parsing
- **Boundary:** FastAPI Microservice
- **Builds:** FastAPI setup, endpoint to accept PDF/DOCX file uploads, extract raw text, and return clean JSON.
- **Dependencies:** None.
- **Done when:** Uploading a PDF via Swagger UI returns the raw extracted text successfully.

### Unit 06: NER Skill Extraction & Vector Generation
- **Boundary:** FastAPI Microservice
- **Builds:** Integration of `spaCy` to extract technical skills (NER) from text, and `Sentence Transformers` to convert those skills into 768-dimensional vector arrays.
- **Dependencies:** Unit 05.
- **Done when:** The FastAPI endpoint takes raw text and returns a JSON payload containing `extracted_skills: [...]` and `vector_embedding: [...]`.

## Phase 3: The Core Matching Engine

### Unit 07: Django Vector Database Schema (`pgvector`)
- **Boundary:** Django Backend
- **Builds:** Job models, User Profile models, and the `pgvector` database migration to store embedding arrays.
- **Dependencies:** Unit 01.
- **Done when:** `python manage.py migrate` succeeds and vectors can be manually inserted into the database.

### Unit 08: Django <-> FastAPI Integration (The Core Loop)
- **Boundary:** Django Backend
- **Builds:** The API endpoint that receives a resume from the user, sends the file to FastAPI, receives the vectors/skills back, and saves them to the user's profile in Postgres.
- **Dependencies:** Unit 06, Unit 07.
- **Done when:** Uploading a file to Django successfully updates the user's database record with AI-generated vectors.

### Unit 09: Semantic Match & Skill Gap Algorithm
- **Boundary:** Django Backend
- **Builds:** The matching logic that queries `pgvector` using cosine similarity to find jobs matching the user's vector, and computes the difference to generate a "Missing Skills" report.
- **Dependencies:** Unit 08.
- **Done when:** Querying the match endpoint returns a ranked list of jobs and explicit missing skills.

## Phase 4: Data Visualization & Dashboard

### Unit 10: Frontend Resume Upload UI & AI Loading States
- **Boundary:** React Frontend
- **Builds:** File upload component, React Query mutation to upload the resume to Django, and skeleton loading states while the AI processes the file.
- **Dependencies:** Unit 04, Unit 08.
- **Done when:** A user can upload a file in the UI and see a loading state until the backend confirms processing.

### Unit 11: Frontend Match Results & Analytics UI
- **Boundary:** React Frontend
- **Builds:** Glassmorphic job match cards and bento-box skill gap charts (using Recharts or Chart.js) populating data from the semantic matching API.
- **Dependencies:** Unit 09, Unit 10.
- **Done when:** The dashboard renders visual job matches and a clear "Skills to Learn" chart.

## Phase 5: Production Peripherals

### Unit 12: PayMongo Subscription Integration
- **Boundary:** Django & React
- **Builds:** Django checkout endpoints, webhook receiver, and Frontend PayMongo checkout flow for premium platform access.
- **Dependencies:** Unit 11.
- **Done when:** A user can complete a test transaction in the UI and their database record upgrades to "Premium".

### Unit 13: Celery Background Alerts Setup
- **Boundary:** Django Backend
- **Builds:** Redis configuration, Celery worker setup, and an async task that fires an email alert when a new job matches a user's vector above 90%.
- **Dependencies:** Unit 09.
- **Done when:** Adding a matching job to the database triggers a background Celery log confirming an email was successfully sent.
