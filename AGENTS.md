# Repository Guidelines

## Project Structure & Module Organization
This repository contains the web app, processing backend, and MCP client:
- `backend/`: FastAPI + async worker code (`src/api`, `src/services`, `src/repositories`, `src/workers`).
- `frontend/`: main Next.js app (`src/app`, `src/components`, `src/lib`, `prisma/`).
- `mcp/`: Python MCP server that connects to the REST API.
- `backend/src/media/`: transcription, captions, timeline, reframing, and ffmpeg helpers.

Infra and bootstrap files live at the root: `docker-compose.yml`, `init.sql`, `.env.example`, and `start.sh`.

## Build, Test, and Development Commands
Use Docker for full-stack development:
- `docker-compose up -d --build`: start frontend, backend, worker, Postgres, and Redis.
- `docker-compose logs -f`: stream service logs.
- `docker-compose down`: stop everything.

Local app commands:
- `cd frontend && pnpm run dev`: run Next.js in dev mode.
- `cd frontend && pnpm run build && pnpm run start`: production build + serve.
- `cd frontend && pnpm run lint`: run ESLint.
- `cd backend && uv sync && uvicorn src.main:app --reload --host 0.0.0.0 --port 8000`: run API locally.
- `cd backend && .venv/bin/arq src.workers.tasks.WorkerSettings`: run the worker.

## Coding Style & Naming Conventions
- Python: 4-space indentation, type hints where practical, `snake_case` for functions/modules.
- TypeScript/React: 2-space indentation, `PascalCase` for component names, `camelCase` for variables/functions, route files in Next.js App Router conventions (`app/.../page.tsx`, `route.ts`).
- Linting: Next.js ESLint configs in `frontend/eslint.config.mjs`.
- Imports: use the `@/*` alias in Next.js apps when possible.

## Testing Guidelines
Automated coverage uses pytest, Vitest, and Playwright:
- Run `make check` for frontend lint and TypeScript checks.
- Run `make test` for backend and frontend unit/integration tests. Backend integration tests need a dedicated Postgres database.
- Run `make test-e2e` for the database-backed browser smoke tests.
- Run `cd frontend && pnpm run test:polish` for isolated browser workflow tests with mocked APIs.
- Install frontend dependencies with `pnpm install --frozen-lockfile`; use the committed pnpm lockfile.
- Smoke test core flows with `docker-compose` (create task, process clips, view task page).

When adding tests, place them near code or under `tests/` with clear names (`test_*.py`, `*.test.ts[x]`).

## Commit & Pull Request Guidelines
Recent history favors short imperative commit subjects (`Add list endpoint`, `Fix typo`, `improve UX`). Prefer:
- `type(scope): concise summary` (example: `feat(backend): add task list pagination`).
- One logical change per commit.

PRs should include:
- What changed and why.
- Any env/config or migration impact.
- Screenshots/GIFs for UI changes.
- Linked issue(s) and manual verification steps.

## Security & Configuration Tips
- Never commit real secrets; use `.env.example` as the template.
- Preserve the committed dependency lockfiles and patched base-image versions when deploying. Run the `Image security` checks against freshly built final images; do not bypass a failing critical-vulnerability check with a server-only tag or an unpinned package install.
- Before a production restart, check that no SupoClip tasks are queued or processing and the worker has no ongoing jobs; let active clips finish first.
- Required runtime keys include `ASSEMBLY_AI_API_KEY` and either one hosted LLM provider key (`OPENAI_API_KEY`, `GOOGLE_API_KEY`, `ANTHROPIC_API_KEY`, or `OPENROUTER_API_KEY`) or an Ollama model configuration (`LLM=ollama:*`, optional `OLLAMA_BASE_URL`).

## Fork & Upstream Sync Guidelines
This repository is a customized fork maintained by `vmellos` based on `FujiwaraChoki/supoclip`.
Coding agents assisting with repository operations, updates, or maintenance must follow these rules:

### Remotes Convention
- `origin`: `git@github.com:vmellos/supoclip.git` (user's fork repository; all `git push` commands MUST target `origin`).
- `upstream`: `git@github.com:FujiwaraChoki/supoclip.git` (official repository; read-only source for features and hotfixes).

### Upstream Sync Protocol
When asked to sync, pull updates, or integrate hotfixes from upstream:
1. **Working Tree Safety**: Run `git status` to verify there are no dirty uncommitted files before initiating any sync.
2. **Fetch Upstream**: Run `git fetch upstream` to get the latest commits and tags.
3. **Inspect Incoming Changes**: Run `git log HEAD..upstream/main --oneline` to review what commits are incoming and check for touched files.
4. **Syncing `main`**:
   - Checkout `main`: `git checkout main`.
   - Merge upstream changes: `git merge upstream/main`.
   - Validate integrity: Run `make check` (frontend lint/TS) and backend tests where applicable.
   - Push to personal fork: `git push origin main`.
5. **Syncing Custom/Feature Branches**:
   - If user adjustments live on a custom branch (e.g., `custom` or `feat/*`), switch to that branch: `git checkout <branch>`.
   - Integrate updates: `git merge main` (or `git merge upstream/main`).
   - If conflicts arise: protect user customizations, preserve custom logic/features, resolve conflict markers, and verify before committing.
   - Push to fork: `git push origin <branch>`.
6. **Guardrail**: Never attempt to push to `upstream`. All changes belong in `origin`.

### Standard Daily Workflow
```bash
# 1. No dia a dia, você trabalha e roda o projeto na branch custom:
git checkout custom

# 2. Quando quiser atualizar com novidades do criador original:
git checkout main
git merge upstream/main
git push origin main

# 3. Incorpora as novidades na sua branch de ajustes:
git checkout custom
git merge main
git push origin custom
```
