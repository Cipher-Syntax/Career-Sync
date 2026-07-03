# AI Workflow Rules

These rules dictate how you, as an AI coding agent, must operate within this codebase. Do not treat these as suggestions. They are strict directives.

## 1. Overall Approach
- **Be Spec-Driven:** Always read `project-overview.md`, `architecture.md`, and `code-standards.md` before making any major structural decisions. Conform your output to these specifications.
- **Work Incrementally:** Implement features step-by-step. Do not attempt to build the entire frontend, backend, and AI pipeline in a single massive prompt or turn.

## 2. Scoping Rules
- **One Unit at a Time:** Focus exclusively on the specific task requested by the user. If asked to build a Django model, do not preemptively build the React component for it.
- **No Speculative Changes:** Do not refactor unrelated code, bump dependency versions, or "clean up" adjacent files unless explicitly instructed to do so.

## 3. When to Split Work
- **Break Down Complexity:** If a requested feature involves modifying multiple distinct domains (e.g., changing the React UI, updating the Django API, and modifying the FastAPI NLP model simultaneously), stop and ask the user which piece to tackle first.
- **Halt on Large Refactors:** If your proposed solution requires rewriting an entire file or significantly changing the architecture, propose the split steps to the user before executing.

## 4. Handling Ambiguity
- **Ask, Do Not Guess:** If a requirement is missing, underspecified, or contradicts existing architecture documents, stop immediately. Ask the user for clarification before writing any code.
- **Fail Loudly:** If a terminal command fails or a dependency error occurs, do not silently apply obscure workarounds. Explain the error and propose the correct, root-cause fix.

## 5. Protected Files
Do not modify the following files or directories unless explicitly instructed:
- Any generated UI components (e.g., components inside `frontend/src/components/ui/` if using shadcn/ui or similar libraries). Treat these as read-only.
- Auto-generated database migration files in Django (`backend-api/*/migrations/`).
- Documentation files (`project-overview.md`, `architecture.md`, `code-standards.md`) unless the user explicitly asks you to update them.

## 6. Keeping Documentation in Sync
- **Update on Change:** Whenever you execute an explicit structural change that alters the data model, system boundaries, or core API routes, you must proactively update the relevant README or architectural documentation to reflect the new state.

## 7. Verification Checklist
Before declaring a unit of work "done" and ending your turn, you must verify the following:
- [ ] Code strictly adheres to `code-standards.md`.
- [ ] No speculative or out-of-scope changes were included in the edit.
- [ ] The code compiles and runs without syntax errors.
- [ ] The relevant components or endpoints handle missing data and edge cases gracefully.
- [ ] You have provided the user with a concise summary of what was actually changed.
