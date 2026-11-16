# Repository Map

Describes the repository as it is on disk. Update when the layout changes. Excludes `.git/` and IDE/OS noise.

## Application source code

None yet. The application is still at the planning stage (see `PROGRESS.md`). When code is added, document its directories here.

## Tests

None yet.

## Configuration and infrastructure

None yet. Planned (per `brainstorm-001.md`): Kubernetes manifests, Dockerfile, build files. Not created.

## Documentation

| Path | Purpose |
|---|---|
| `README.md` | Project README: project TODO list, plus a short framework section. |
| `brainstorm-001.md` | Project specification and roadmap (German). |
| `AI_DEV_FRAMEWORK_README.md` | Earlier German quick-start for the framework. Overlaps with the README framework section; see `PROGRESS.md` open questions. |
| `research.md` | Earlier research report on the project. |

## AI framework files

| Path | Purpose |
|---|---|
| `AI.md` | Shared entry point for all AI agents. |
| `GUIDE.md` | Collaboration and learning protocol. |
| `AI_RULES.md` | Rules for AI behaviour (developer-approved only). |
| `RULES.md` | Project quality rules. |
| `TREE.md` | This file. |
| `PROGRESS.md` | Verified project state. |
| `CLAUDE.md` | Thin adapter for Claude Code; points to `AI.md`. |
| `AGENTS.md` | Thin adapter for agents that read `AGENTS.md`; points to `AI.md`. |
| `ai_dev_framework_prompt.md` | Prompt that generated the framework. Reference only. |
| `notes/` | Developer knowledge notes. `README.md`, `TEMPLATE.md`. |
| `patterns/` | Pattern practice material. `README.md`, `TEMPLATE.md`. |
| `docs/decisions/` | Architecture decision records. `TEMPLATE.md`. |
| `.scratch/drafts/` | AI reference implementations. Git-ignored. Empty. |
| `.scratch/reviews/` | AI review artifacts. Git-ignored. Empty. |

## Agent and tool configuration

| Path | Purpose | Tracked in git |
|---|---|---|
| `.claude/skills/guideme/SKILL.md` | `guideme` skill for Claude Code (implementation guide, no code). | yes |
| `.pi/agent/skills/guideme/SKILL.md` | Same skill for the Pi agent. | yes |
| `.claude/` (other files) | Claude Code local settings, hooks, routines. Environment-managed. | no (untracked) |
| `.idea/` | IntelliJ IDEA project files. | no (untracked) |
| `.vscode/`, `.bashrc`, `.zshrc`, `.gitconfig`, `.mcp.json` and similar | Environment files. Not documented here. | no (untracked) |

## Learning resources

`notes/`, `patterns/` (see above). Both start empty.

## Conventions

- Framework documents are in English so every agent can read them. Project and personal documents (`README.md`, `brainstorm-001.md`, `AI_DEV_FRAMEWORK_README.md`) remain in German.
- Temporary AI artifacts go only into `.scratch/`.
- Application code, when it exists, goes in directories chosen for that project. Do not force `src/` or `tests/` onto it.
