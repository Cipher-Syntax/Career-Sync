# CareerSync: Project Overview

## Overview
CareerSync is an AI-powered employment platform that modernizes the job search process by leveraging Natural Language Processing (NLP) and embedding techniques. Instead of relying on rigid keyword matching, the system understands the semantic meaning behind a user's skills and experience. By analyzing uploaded resumes (PDF/DOCX) against a vector database of job descriptions, CareerSync provides users with highly accurate job recommendations, highlights specific missing technical skills, and alerts them to new opportunities—ultimately bridging the gap between job seekers and their ideal roles.

## Goals
1. Automate the parsing and extraction of technical skills from user-uploaded resumes.
2. Implement semantic similarity matching using transformer-based embeddings and PostgreSQL (`pgvector`) to increase recommendation accuracy.
3. Provide actionable skill gap analysis so users know exactly which technologies they need to learn to qualify for their target roles.
4. Establish a secure and premium user experience featuring JWT + OAuth authentication and a modern, glassmorphic UI.
5. Create a scalable architecture by isolating heavy NLP processing in a dedicated microservice away from the primary web application.

## Core User Flow
1. **Sign Up / Login:** The user accesses the platform and logs in using LinkedIn/Google OAuth or standard email/password (with OTP verification).
2. **Subscription / Checkout:** The user completes a payment gateway flow via PayMongo to access premium features.
3. **Resume Upload:** The user uploads their current resume in PDF or DOCX format.
4. **Processing:** The system securely passes the document to the AI microservice, which extracts text, performs Named Entity Recognition (NER) to isolate skills, and generates dense vector embeddings.
5. **Dashboard Generation:** The user is presented with a personalized dashboard containing:
   - A list of highly compatible job matches with percentage scores.
   - External links to apply for those specific jobs.
   - A visual report (e.g., charts) detailing their skill gaps compared to the market requirements.
6. **Alerts (Ongoing):** The user receives asynchronous email notifications via Celery/Redis when new jobs are added to the database that match their profile.

## Features by Category

### Authentication & Security
- Google and LinkedIn Single Sign-On (SSO).
- JWT-based authentication for decoupled React-Django architecture.
- Email verification using One-Time Passwords (OTP).

### AI & Natural Language Processing
- Automated text extraction from PDF and DOCX files.
- Named Entity Recognition (NER) for skill categorization via spaCy.
- Generation of dense vector embeddings using Sentence Transformers (PyTorch).
- Semantic job matching based on cosine similarity calculations.

### Data & Architecture
- Vector storage and querying using PostgreSQL and `pgvector`.
- Microservice architecture separating the Django core from the FastAPI NLP engine.
- Asynchronous task processing for email alerts using Celery and Redis.

### Frontend & User Experience
- Premium dark-mode interface with glassmorphism and electric blue/teal accents.
- Responsive, bento-box grid layouts for analytics and charts.
- Dynamic data fetching and caching using React Query.
- Secure payment processing via PayMongo integration.

## In-Scope
- Building the React + TailwindCSS frontend application.
- Developing the Django REST Framework main backend API.
- Developing the FastAPI + PyTorch NLP microservice.
- Implementing OAuth, JWT, and OTP authentication.
- Integrating PayMongo for subscription/payment handling.
- Sourcing initial job data via static datasets (e.g., Kaggle) or reliable Job APIs.
- Background email alerting system for new job matches.

## Out-of-Scope
- A visual, drag-and-drop ATS-compliant Resume Builder.
- Real-time, live web scraping of external job boards like LinkedIn or Indeed.
- In-platform applicant tracking portals for employers and recruiters.
- Training large language models (LLMs) from scratch (utilizing pretrained models instead).

## Success Criteria
- **Authentication:** Users can successfully log in via OAuth or Email/OTP and establish a secure JWT session.
- **Processing:** Resumes are accurately parsed, with a minimum of 85% of explicit technical skills successfully extracted.
- **Matching:** The system returns job recommendations in under 3 seconds per query, leveraging `pgvector` indexing.
- **Stability:** The main Django web server never crashes or hangs due to heavy NLP workloads, proving successful microservice isolation.
- **UI/UX:** The frontend dashboard loads dynamically without full page refreshes, properly rendering the skill gap analytics and job cards.
- **Payments:** PayMongo successfully processes transactions and grants access to premium platform features.
