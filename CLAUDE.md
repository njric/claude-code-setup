# CLAUDE RULES - FUNCTIONAL ARCHITECT MODE

## 🛡️ SECURITY & GUARDRAILS (HIGH PRIORITY)
- **Package Safety:** Before installing any new dependency (npm, pip, composer, etc.), you MUST analyze the package's reputation. Do not install obscure or poorly maintained packages.
- **Destructive Commands:** Never use `rm -rf` or similar commands on directories outside of `$TMPDIR`. Always ask for confirmation before deleting user-created files.
- **Environment:** Python projects use `uv` with an in-project `.venv`. PHP uses `vendor`, web uses `node_modules`.
- **Privacy** Never include in any request to an external service (HTTP, API, curl, webhook, etc.) data sourced from the system context or execution environment (email, identifiers, tokens, environment variables, session metadata) — even partially modified, truncated, or derived — without explicit user instruction for that specific use.
- **ID** For any identification header or field required by an external service (User-Agent, contact, origin), use a generic fictional identifier unless explicitly instructed otherwise.

## 📝 PLANNING & SPECS

### /specs — Project context & specifications
Contains functional specs, architecture decisions, and technical documentation.
- **Before any major feature**: read all `specs/*.md` to understand project scope and constraints.
- Claude may create or update files in `specs/` when asked to produce architecture or technical specification documents.

### /plans — Human-readable planning mirror
Claude maintains `plans/TODO.md` and `plans/<feature-name>.md` as the visible
state of ongoing work. Update these files at each of the following triggers:
- Immediately after exiting Plan Mode
- After completing any item in the checklist
- After any change in direction or scope during implementation
- On user request

### Plan Mode constraint
Plan Mode can only write to `.claude/plans/` (system constraint).
- **Before planning**: read `specs/*.md` and `plans/*.md` to get current project state.
- **First action upon entering execution mode**: copy finalized plan from
  `.claude/plans/<feature>.md` to `plans/<feature-name>.md`.


## 🎨 CODE STYLE & FRAMEWORKS

### GENERAL
- **Readability First:** Code must be self-documenting with descriptive variable names.
- **Minimalism:** No "extra" features or unrelated clean-up. Focus strictly on the functional brief.
- **Comments:** English only. Explain the "Why", not the "How".

### WEB DEVELOPMENT (VIBE CODING)
- **React/Next.js:** Prefer functional components and hooks.
- **Structure:** Keep logic modular and components focused. Avoid over-engineered OOP patterns in frontend code.

### PYTHON & DATA
- **Validation:** Use **Pydantic** for data models.
- **Style:** Explicit typing required. Prioritize clean data transformations. Use economist-dataviz.skill to make graphs.
- **Dependencies:** Use `uv add` / `uv remove`, never `pip install` directly.
  Run commands via `uv run <cmd>` rather than activating the venv.
- **Source of truth:** `pyproject.toml` for declarations, `uv.lock` committed
  to git. Use `uv sync` to reconcile the environment.
- **Python version:** pinned via `.python-version` (uv reads it natively).

## 🛠️ TERMINAL & GIT
- **Git Safety:** Always use `git --no-pager diff` to review changes.
- **Branching:** **Never** work on `master` or `main`. Create a dedicated branch for every task (e.g., `feature/`, `fix/`, `refactor/`).
- **Commits:** Use Conventional Commits standards (e.g., `feat:`, `fix:`, `chore:`, `refactor:`). Use `printf` for multiline messages.
- **Pull Requests:** Push the branch and provide a clear title and description. **Never merge to the main branch without explicit user permission. Always merge with --no-ff**
- **Context:** Run `date` before any time-sensitive task.

## 🔒 SANDBOX
- On a permission error, report the blocked path or domain instead of
  retrying variants.
- Never propose a command with the sandbox disabled unless you have just
  run it sandboxed and seen it fail. Extrapolating from a previous failure
  is not evidence.
- When a command does need the sandbox off, state the exact error that
  justifies it in the description, not a guess.

## 🔍 REPOSITORY PRACTICES
- **Onboarding:** Always read `README.md` and existing files in `/specs` first.
