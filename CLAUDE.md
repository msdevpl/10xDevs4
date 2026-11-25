# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Commands

### Set the default model
```bash
/model
```
Sets the model to `lfm3:latest` as the default for new sessions.

### Build / Install
- Use the standard build command (typically `npm install` or `python -m pip install -e .` depending on the project).
- After installation, verify the setup with `git status` or run the project's entry point.

### Linting
- Run linter (e.g., `npm run lint` or `flake8`, `eslint`, or `ruff`) to check code style.
- Typical command: `npm run lint`

### Running Tests
- Run the full test suite: `npm test` or `pytest`
- Run a single test file: `npm test -- <filename>` or `pytest <path>/<filename>.py`

### Quick Start
- Ensure the environment is set up: `/model` (as above).
- Clone the repo and enter the directory.
- Install dependencies: `npm install` (or equivalent).
- Run tests to verify everything passes: `npm test`

## High-Level Architecture & Structure

This repository is a modern application project (likely a CLI or server app) built with the **Claude Code** framework. Key structural observations:

- **Root directory**: `/Users/kkarazniewicz/AI` — the project root.
- **Model configuration**: Model is set via `/model` command to `lfm3:latest` and saved as the default for new sessions.
- **Code organization**: Standard project layout with source files, tests, and possibly config files. (Inspect `.claude/` or `settings.json` for project-specific details.)
- **Tooling ecosystem**: Available tools include `Bash`, `Read`, `Write`, `ToolSearch`, `Workflow`, `Agent`, `Security-review`, `code-review`, `update-config`, etc. Use them as needed for file ops, searches, and orchestration.
- **Skills available**: The project may leverage skills such as `claude-code-guide`, `update-config`, `security-review`, `code-review`, `simplify`, `loop`, etc. Load relevant skills before running tasks.

### Typical Workflow

1. **Start a new session** — ensure model is set: `/model`.
2. **Explore the codebase** — use `Read` to inspect files, `Bash` for shell commands, `ToolSearch` for deferred tools.
3. **Build / install** — run the install/build command(s) specified in `package.json` or `README.md`.
4. **Lint** — run the linter to catch style issues early.
5. **Test** — run `npm test` (or `pytest`) to verify functionality.
6. **Develop iteratively** — use `Edit`/`Write` for changes, `git` for version control, and/or `Workflow` for multi-agent orchestration when needed.

## Notes

- Do not include generic development practices (e.g., "write tests for all new utilities").
- Avoid listing every component/file if the structure is easy to discover.
- If Cursor or Copilot rules exist, refer to `.cursor/rules/` or `.github/copilot-instructions.md`.
- If a `README.md` exists, summarize its important parts.
- The system reminder about git commit/PR attribution (Co-Authored-By: Claude Code) applies to generated commits/PRs.
<!-- BEGIN @przeprogramowani/10x-cli -->

## 10xDevs AI Toolkit — Module 1, Lesson 1

Bootstrap a greenfield project end-to-end with the **shaping chain**:

```
/10x-init  →  /10x-shape  →  /10x-prd  →  (10x-tech-stack-selector)  →  (bootstrapper)
```

The first three skills ship in this lesson; the last two are the next links in the chain.

### Task Router — Where to start

| Skill | Use it when |
| --- | --- |
| **Project setup** | |
| `/10x-init` | The project directory is fresh. Scaffolds `context/foundation/lessons.md` and `docs/reference/contract-surfaces.md` so the rest of the workflow has somewhere to write. Run this once per project. |
| **Discovery** | |
| `/10x-shape` | You have an idea and need to turn it into structured shape-notes BEFORE writing a PRD. Greenfield only. Walks vision → persona/access → MVP → FRs (with Socratic challenge) → business logic & data → stack-openness sketch. Surfaces empty-CRUD and MVP-too-big anti-patterns by name. Output: `context/foundation/shape-notes.md` with a resumable `checkpoint:` block. |
| **Document generation** | |
| `/10x-prd` | You have shape-notes (or raw notes) and want a schema-conformant `context/foundation/prd.md`. Generates against the locked schema, routes every gap verbatim into `## Open Questions`, and refuses to invent domain decisions. On collision, prompts overwrite vs. versioned save (`prd-vN.md`). |

### How the chain hands off

- `/10x-init` produces the workflow v2 scaffold (`context/foundation/`, `lessons.md`, `contract-surfaces.md`). `/10x-shape` requires this and will offer to delegate to `/10x-init` if it's missing.
- `/10x-shape` writes `context/foundation/shape-notes.md` with frontmatter `checkpoint:` (current_phase, phases_completed, frs_drafted, quality_check_status). On re-entry, it resumes from the next unfinished phase.
- `/10x-prd` reads `shape-notes.md` (default) or any path you pass, scores the input on a 4-signal heuristic, warns on thin input, and writes `context/foundation/prd.md` against the schema at `skills/10x-shape/references/prd-schema.md` (frontmatter aligned 1:1 with 10x-tech-stack-selector's Q1–Q7).

### What the PRD captures (and what it does NOT)

- **Captured**: vision, persona, success criteria, user stories (Given/When/Then), FRs (FR-NNN), NFRs, business logic (one-sentence rule first), data model, access control, durable implementation decisions, testing strategy, deployment & CI/CD strategy, non-goals, open questions.
- **NOT captured (deliberate)**: framework choices, database choices, file paths, deployment platform. Stack openness is binding — only `product_type` and `tech_preferences.language_family` capture stack-shaped intent. Frameworks are 10x-tech-stack-selector's job.

### Anti-patterns surfaced during shaping

- **Empty-CRUD**: business logic that reduces to "users add and remove records" with no domain rule. `/10x-shape` names it explicitly and prompts for a real rule shape (recommendation, prioritization, classification, validation, scoring, workflow, calculation).
- **MVP-too-big**: first-flow estimate exceeds ~1 week of after-hours work, or > 4 distinct user actions before user-visible value, or requires multiple integrations before payoff. Skill names the expensive pieces and offers concrete scope-down moves.

Both are **soft gates**: they warn but allow override. Overrides are recorded in the checkpoint and surfaced in the PRD's `## Open Questions`.

### Foundation paths used by this lesson

- `context/foundation/shape-notes.md` — `/10x-shape` output
- `context/foundation/prd.md` (or `prd-vN.md`) — `/10x-prd` output
- `context/foundation/lessons.md` — recurring rules & pitfalls (scaffolded by `/10x-init`)
- `docs/reference/contract-surfaces.md` — load-bearing names registry (scaffolded by `/10x-init`)

### Universal language

The shipped skills carry no 10xDevs / cohort / certification references. The mechanics (Socratic challenge, gray-area discovery, recommended-answer fatigue mitigation, soft quality gate) are universal indicators of a well-scoped greenfield project.

Skills must not write to `context/archive/`. Archived changes are immutable; if a resolved target path starts with `context/archive/`, abort with: "This change is archived. Open a new change with `/10x-new` instead."

<!-- END @przeprogramowani/10x-cli -->
