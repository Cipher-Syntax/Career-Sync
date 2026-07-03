# CareerSync Architecture

## Technology Stack

| Layer | Technology | Role |
| :--- | :--- | :--- |
| **Frontend** | React + TailwindCSS | Renders the premium dark-mode UI, glassmorphic elements, and dashboards. |
| **State & Data Fetching** | React Query | Handles caching, loading states, and background refetching for async API calls. |
| **Main Backend** | Django + Django REST Framework | Manages user accounts, payments (PayMongo), business logic, and API endpoints. |
| **AI Microservice** | FastAPI + PyTorch | A lightweight, memory-intensive service dedicated entirely to NLP tasks (parsing, NER, embeddings). |
| **NLP Libraries** | Sentence Transformers, spaCy | Used for entity extraction and converting text into dense vector embeddings. |
| **Database** | PostgreSQL + `pgvector` | Stores relational data (users, jobs) and natively searches vector embeddings using cosine similarity. |
| **Task Queue / Cache** | Celery + Redis | Handles asynchronous tasks like sending email alerts to prevent blocking the main web thread. |
| **Payments** | PayMongo API | Processes user subscriptions and premium feature access securely. |

## System Boundaries

The application is strictly separated into three distinct domains. Each domain lives in its own repository or root folder and has a single responsibility.

- `/frontend/` (React Application)
  - **Responsibility:** Presentation, UI/UX, and data rendering.
  - **Rules:** Owns the visual language. Contains no direct database access. Never holds secure backend keys.
- `/backend-api/` (Django Application)
  - **Responsibility:** Core business logic, authentication, database CRUD operations, and payment handling.
  - **Rules:** Owns the primary PostgreSQL database. Does **not** process complex NLP tasks or load heavy PyTorch models into its memory. Offloads long-running tasks to Celery.
- `/ai-service/` (FastAPI Microservice)
  - **Responsibility:** Text extraction from PDFs, Named Entity Recognition (NER), and embedding generation.
  - **Rules:** Receives text or files via HTTP, returns JSON containing mathematical vectors and extracted entities. Completely stateless. Owns no long-term storage.

## Storage Model

- **PostgreSQL Database:** The single source of truth for structured data.
  - *Standard Tables:* User profiles, subscription statuses, job listings, application history.
  - *Vector Columns (pgvector):* Stores the 768-dimensional (or equivalent) embedding vectors representing user skills and job requirements.
- **File Storage (AWS S3 / Cloudinary):**
  - Stores the actual PDF/DOCX files uploaded by users. The database only stores a URL reference to these files. This keeps the database lightweight.
- **Cache / Message Broker (Redis):**
  - Acts as the message broker for Celery to queue asynchronous tasks (like email notifications).
  - Caches frequent, expensive queries (e.g., popular job searches) temporarily.

## Authentication and Access Model

- **Authentication Strategy:** The system utilizes decoupled JWT (JSON Web Tokens) to authenticate requests between the React frontend and Django backend.
- **Single Sign-On (SSO):** Users can authenticate via Google or LinkedIn OAuth. These integrations rely on Django libraries (e.g., `dj-rest-auth` and `django-allauth`) to map external identities to internal user accounts.
- **Standard Login:** Email and password registration requires OTP (One-Time Password) email verification before full account activation.
- **Ownership & Authorization:** 
  - Every resource (resume, skill gap report) is strictly tied to a user ID.
  - Database queries automatically filter by the authenticated user's ID (`request.user`). Users cannot access or modify resumes or matching reports that do not belong to them.

## AI and Background Task Models

- **AI Processing Pipeline:**
  1. User uploads a resume to the Django API.
  2. Django passes the file URL/content to the FastAPI AI microservice.
  3. FastAPI extracts the text, uses `spaCy` to run Named Entity Recognition (finding specific technical skills).
  4. FastAPI feeds the skills into `Sentence Transformers` to generate vector embeddings.
  5. The vectors and entities are returned to Django, which saves them in PostgreSQL.
- **Background Alert Model:**
  - When new jobs are seeded into the database, a Celery worker computes similarity scores against existing users. If a high-confidence match is found, Celery queues an email alert job, preventing the matching logic from slowing down web requests.

## Invariants

These are strict architectural rules that the codebase must *never* violate:

1. **No Blocking AI in the Main Thread:** The Django backend must never instantiate PyTorch models or run synchronous NLP pipelines directly. All AI workloads must be offloaded to the FastAPI microservice to prevent out-of-memory crashes and thread blocking.
2. **Vectors Stay in Postgres:** All vector similarity searches must be executed natively inside PostgreSQL via `pgvector`. Do not pull thousands of vectors into Python memory to compute cosine similarity manually.
3. **Frontend is Secret-Blind:** The React frontend must never contain secret keys (e.g., PayMongo Secret Keys, Django Secret Key). Only public, publishable keys are permitted in the client bundle.
4. **Stateless AI:** The FastAPI microservice must remain entirely stateless. It must not connect directly to the PostgreSQL database to read or write data; it only accepts inputs via HTTP requests and returns outputs to the Django backend.
5. **No File Blobs in the Database:** Uploaded resumes (PDFs/DOCX) must never be stored as raw BLOBs/Binaries inside PostgreSQL. They must be stored in external File Storage, with only the URL saved in the database.
