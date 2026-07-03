<div align="center">

<h1><strong>CareerSync</strong></h1>

**An AI-Powered Resume Analysis and Job Recommendation Platform**

<br />

[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](#)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](#)
[![Django](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)](#)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](#)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](#)
[![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)](#)

<br />

*Eliminating keyword-matching. Connecting talent with opportunity through deep semantic understanding.*

</div>

---

## Overview

Traditional job hunting is broken. Recruiters and job platforms rely on rigid keyword-matching systems that fail to understand the true semantic relationship between technical skills and professional experience. 

**CareerSync** is a modern, AI-powered recruitment engine. By leveraging Natural Language Processing (NLP) and transformer-based embeddings, CareerSync understands the actual context of a user's skills. It parses uploaded resumes, extracts technical competencies, and matches them against thousands of job descriptions using dense vector similarity.

Instead of just showing jobs, CareerSync provides **actionable intelligence**: telling job seekers exactly what skills they are missing to qualify for their dream roles.

## Key Features

- **Intelligent Resume Parsing:** Automatically extracts structured text and identifies technical skills from uploaded PDF and DOCX resumes using Named Entity Recognition (NER).
- **Semantic Job Matching:** Goes beyond keywords. Uses `Sentence Transformers` and `pgvector` to find jobs that mathematically align with your true skill set.
- **Skill Gap Analytics:** Visualizes exactly which required technologies are missing from your resume, providing a clear roadmap for upskilling.
- **Asynchronous Alerts:** Background processing ensures you are instantly notified via email when new, highly compatible jobs are added to the platform.
- **Enterprise-Grade Security:** Fully decoupled architecture featuring OAuth (Google/LinkedIn), JWT authentication, and Email OTP verification.

## System Architecture

To guarantee scalability and stability, CareerSync is divided into three highly specialized domains:

1. **Frontend (React + TailwindCSS):** A premium, glassmorphic UI built for speed, relying on React Query for seamless data caching.
2. **Core API (Django + Celery):** Handles secure authentication, user management, background email alerts, and payment processing (via PayMongo).
3. **AI Microservice (FastAPI + PyTorch):** A dedicated, stateless NLP engine that safely handles heavy machine learning workloads (Entity extraction and Embedding generation) without bringing down the main web server.

## Getting Started

*(Documentation on how to run this project locally will be added as the infrastructure is initialized.)*

### Prerequisites
- Node.js (v18+)
- Python (3.10+)
- PostgreSQL (with `pgvector` extension enabled)
- Redis (for Celery workers)

## Contributing

We welcome contributions to CareerSync! Please ensure you read our `code-standards.md` and `ai-workflow-rules.md` before submitting a Pull Request.

---
<div align="center">
  Built with passion for the future of work.
</div>