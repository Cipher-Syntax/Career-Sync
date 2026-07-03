# Code Standards

## General

- Keep modules small and single-purpose. If a file exceeds 300 lines, consider refactoring it into smaller components or utility functions.
- Fix root causes, do not layer workarounds. If a bug occurs, track it down to its origin rather than patching the symptom.
- Do not mix unrelated concerns. AI logic stays in the AI service, business logic in Django, and UI presentation in React.

## JavaScript / React

- Use modern ES6+ features (arrow functions, destructuring, optional chaining).
- Avoid relying on global state unless absolutely necessary. Rely on React Query for server state and local React state for UI state.
- Validate unknown external input at system boundaries before trusting it (e.g., using a library like Zod or Yup before sending forms).
- Keep components pure where possible. Side effects should be strictly managed inside `useEffect` or React Query hooks.

## Django & Django REST Framework

- Keep views/viewsets as thin as possible. Offload complex business logic to separate service layers or model methods.
- Never block the main thread. Always offload long-running tasks (email sending, AI calls) to Celery.
- Use explicit Django serializers for all incoming and outgoing data to ensure validation and consistency.
- Avoid N+1 query problems. Always use `select_related` or `prefetch_related` when accessing foreign keys in lists.

## FastAPI & AI Microservice

- Keep the API strictly stateless. Do not connect to the PostgreSQL database from this service.
- Use Pydantic models for rigorous request and validation of incoming data.
- Load large models (e.g., PyTorch, Sentence Transformers) into memory once during application startup, not on every request.

## Styling

- Use TailwindCSS utility classes exclusively. Do not write custom CSS in standalone files unless absolutely unavoidable.
- Rely on Tailwind's design token scale (e.g., `text-sm`, `rounded-lg`)—no hardcoded pixel values.
- Enforce dark mode compatibility on all new components using the `dark:` variant prefix.

## API Routes

- Validate and parse request input before any logic runs. Reject malformed requests with a 400 Bad Request immediately.
- Enforce auth and ownership before any mutation. Ensure `request.user` has permissions to modify the requested resource.
- Return consistent, predictable response shapes (e.g., wrapping lists in `{ "results": [...] }`).

## Data and Storage

- Standard relational data and `pgvector` embeddings belong in the PostgreSQL database.
- Large generated content and user uploads (like PDF/DOCX resumes) belong in file/blob storage (AWS S3/Cloudinary), with only the URL saved in the database.
- Do not store large binary blobs or Base64 strings directly in the database.

## File Organization

- `frontend/` — The React application. Contains `src/components/`, `src/hooks/` (React Query), `src/pages/`, and Tailwind configuration.
- `backend-api/` — The Django project. Contains core apps (e.g., `users/`, `jobs/`, `payments/`), Celery task configurations, and DRF serializers.
- `ai-service/` — The FastAPI microservice. Contains PyTorch model loading scripts, NLP processing logic, and Pydantic schemas.
- `docs/` — Documentation, architectural decisions, and coding standard files.
