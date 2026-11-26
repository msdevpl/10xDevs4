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

## 10xDevs AI Toolkit — Module 1, Lesson 2

Pick a starter and a stack for the PRD you wrote in Lesson 1, with the **stack chain**:

```
(/10x-init  →  /10x-shape  →  /10x-prd)  →  /10x-tech-stack-selector  →  (bootstrapper)
```

The PRD chain ships from Lesson 1 (re-included in this lesson so you can fix the PRD mid-flight). `/10x-tech-stack-selector` is the lesson's main topic; `/10x-bootstrapper` is the next link, taught in Lesson 3.

### Task Router — Where to start

| Skill | Use it when |
| --- | --- |
| **Stack selection (lesson focus)** | |
| `/10x-tech-stack-selector` | You have a PRD at `context/foundation/prd.md` and need to pick a starter. Opens with an explicit choice (take the recommended default for your `(product_type, language_family)` cell, or design your own), walks the follow-up question set when you design your own, applies four agent-friendly quality gates, reasons over the language-aware starter registry, and writes `context/foundation/tech-stack.md`. Optional `[path-to-prd]` argument lets you point at a non-default PRD location (e.g., `/10x-tech-stack-selector @context/foundation/prd-v2.md`); without it the skill defaults to `context/foundation/prd.md`. Use AFTER `/10x-prd`, BEFORE `/10x-bootstrapper`. |
| **Re-run upstream if needed** | |
| `/10x-init` / `/10x-shape` / `/10x-prd` | Bundled so you can fix the PRD mid-flight. If `/10x-tech-stack-selector` surfaces a gap (e.g., a Functional Requirement that forces a feature your recommended starter doesn't carry), re-run `/10x-prd` to amend the PRD before the stack pick. |

### How the chain hands off

- `/10x-tech-stack-selector` reads `context/foundation/prd.md` frontmatter (`product_type`, `target_scale`, `timeline_budget`) as priors. If the PRD is absent, it refuses with a one-sentence redirect to `/10x-shape` — no inline mini-PRD fallback.
- The skill writes `context/foundation/tech-stack.md` with a 4-key frontmatter (`starter_id`, `package_manager`, `project_name`, `hints`) plus a one-paragraph `## Why this stack` body. The hand-off is intentionally minimal — bootstrapper does not parse rationale, only fields.
- `/10x-bootstrapper` (Lesson 3) reads `tech-stack.md` and the registry to scaffold the project.

### What tech-stack-selector captures (and what it does NOT)

- **Captured**: starter pick (registry-shaped), language family, package manager (open string per ecosystem — `pnpm`, `uv`, `bundle`, `cargo`, etc.), team size, deployment target (drawn from the chosen starter's `deployment_defaults`), CI/CD provider + flow, bootstrapper confidence (`verified | first-class | best-effort`), path taken (standard | custom), self-check answers (custom path), quality override (set when the user proceeds with a starter that failed ≥1 agent-friendly gate), feature flags (auth/payments/realtime/AI/background-jobs).
- **NOT captured (deliberate)**: strategic test plan, strategic deployment plan, strategic implementation decisions. Those are downstream of stack selection — a future technical-roadmap concern, not yet planned. Tech-stack-selector owns *framework-shaped* test/deploy/CI choices because those are inseparable from stack pick; what defers is the *strategic* layer ("we TDD on X surface", "preview environment per PR").

### The opening choice (load-bearing)

The first question is an explicit choice — never silent. The skill names the recommended starter for your `(product_type, language_family)` cell up front and asks for explicit confirmation:

- **Standard path** — accept the recommended default. The skill skips the feature audit, team profile, tech preferences, and framework-variant questions; it asks only the deployment, CI/CD, and project-name questions. The hand-off records `path_taken: standard` under `hints`.
- **Custom path** — design your own. The skill walks the full follow-up set (feature audit, team profile, tech preferences, deployment, CI/CD, framework variant), drills into a testing-runner question only when the chosen starter leaves it ambiguous, and closes with a 5-point readiness self-check (from prework lesson 4.1) before locking in. The hand-off records `path_taken: custom` and populates `self_check_answers`.

The recommended-default-per-cell map is multi-language: web/JS and saas/JS both → 10x-astro-starter (the 10x-branded starter leads whenever it competes in a JS cell); api/JS → hono; api/Python → fastapi; web/Python → django; web/Ruby → rails; api/Go → go; api/Rust → axum; mobile/Dart → flutter; desktop/Rust → tauri; etc. Cells with no vetted default carry `<none>` and force the custom path.

### Quality gates (agent-friendly criteria)

Every starter card carries four booleans the LLM filters against:

1. **Typed** — explicit types/schemas the agent can reason from without running the program.
2. **Convention-based** — strong opinions on layout, routing, configuration.
3. **Popular in training data** — assessed *per language family*, not globally (Django is popular within Python training data; Spring within Java; etc.).
4. **Well-documented** — current, version-pinned, link-able docs.

Candidates failing any gate are excluded from the unprompted recommendation set. If you explicitly name a failing starter as your preference, the skill challenges that pick — surfacing the strongest higher-criteria alternative AND the compensation path (CLAUDE.md instructions that patch the gaps) — and asks you to confirm or pivot. Confirming the known-friction pick records the override on the hand-off so bootstrapper can adjust.

### Bootstrapper confidence

Every recommendation surfaces `bootstrapper_confidence` verbatim — never silently elided:

- **`verified`** — bootstrapper has been run end-to-end on this stack; scaffolding will be smooth.
- **`first-class`** — registered with a valid CLI, expected to work but not battle-tested; expect mostly-smooth scaffolding with occasional manual steps.
- **`best-effort`** — limited support; manual steps likely; expect friction (and bootstrapper's CLAUDE.md generation compensates with extra ecosystem-specific context).

This is the heads-up before running `/10x-bootstrapper` so you know what to expect.

### Foundation paths used by this lesson

- `context/foundation/prd.md` — input (from Lesson 1)
- `context/foundation/tech-stack.md` — output (the chain hand-off)
- `context/foundation/lessons.md` — recurring rules & pitfalls
- `docs/reference/contract-surfaces.md` — load-bearing names registry

### Universal language

The shipped skill carries no 10xDevs / cohort / certification references. The recommended-default registry is multi-language (JS, Python, Ruby, Java, Go, Rust, PHP, .NET, Dart) and the cohort's `10x-astro-starter` is one card in the JS+web cell — not "the" recommended path for everyone.

Skills must not write to `context/archive/`. Archived changes are immutable; if a resolved target path starts with `context/archive/`, abort with: "This change is archived. Open a new change with `/10x-new` instead."

<!-- END @przeprogramowani/10x-cli -->
