# Progress Tracker

Update this file after every meaningful implementation change.

## Current Phase

- Phase 0: Project Setup & Infrastructure Initialization

## Current Goal

- Initialize the foundational structure and repositories for the React frontend, Django backend, and FastAPI microservice.

## Completed

- Created `project-overview.md` defining scope and success criteria.
- Created `architecture.md` defining system boundaries, data flow, and invariants.
- Created `code-standards.md` detailing cross-stack conventions.
- Created `ai-workflow-rules.md` establishing rules for AI agent execution.
- Created `ui-context.md` defining color tokens, typography, and aesthetic goals.

## In Progress

- Initializing the core progress tracker.

## Next Up

- Initialize the Django backend API and configure PostgreSQL with `pgvector`.
- Initialize the React + TailwindCSS frontend application.
- Initialize the FastAPI NLP microservice.

## Open Questions

- None at the moment. (Initial scoping and hosting questions have been resolved).

## Architecture Decisions

- **Microservice NLP:** Decoupled the NLP engine (Sentence Transformers, PyTorch, spaCy) into a stateless FastAPI microservice to protect the main Django web server from heavy RAM usage and blocking tasks.
- **Database:** Chose PostgreSQL with `pgvector` for native vector embeddings storage instead of a separate external vector database to reduce infrastructure complexity.
- **Scope limitation:** Pushed the visual ATS Resume Builder feature to Phase 2 to avoid scope creep for the MVP.
- **Authentication:** Selected decoupled JWT + OAuth (Google/LinkedIn) combined with Email OTP to ensure production-ready security.

## Session Notes

- This is the very beginning of the build phase. All foundational blueprint documents are in place. When resuming an AI session, read `project-overview.md` and `architecture.md` to restore full context, then immediately begin executing the tasks listed in the "Next Up" section.
