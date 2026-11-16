# Project Progress

Only verified progress is recorded here. Planned work is not marked as done. Framework maintenance is tracked separately in `AI_RULES.md` and `RULES.md`.

## Current phase

**Planning.** No application code exists yet.

## Completed (verified)

- Project specification written: `brainstorm-001.md`.
- Repository has three commits on `main` (`Initial commit`, `init`, `ai dev framework prompt and readme added`).
- `guideme` skill present for Claude Code and Pi (`.claude/skills/guideme/`, `.pi/agent/skills/guideme/`).
- Earlier research report written: `research.md`.
- AI development framework initialised (this change): `AI.md`, `GUIDE.md`, `AI_RULES.md`, `RULES.md`, `TREE.md`, `PROGRESS.md`, `notes/`, `patterns/`, `docs/decisions/`, `.scratch/`, `CLAUDE.md`, `AGENTS.md`, `.gitignore`.

## Current task

None in the application. Framework initialisation is complete pending the developer's review.

## Next steps

From `brainstorm-001.md`, mini project steps 0–4:

0. Create broken pod in namespace `playground` (`ImagePullBackOff`).
1. Spring Boot project with `GET /diagnose/{namespace}` (Fabric8).
2. `@Scheduled` watcher with deduplicated logging.
3. Kubernetes manifests: namespace, RBAC, deployment, service.
4. Verify via `port-forward` and `curl`.

Whether to follow the mini project is still open (see below).

## Important decisions

- Learning project: developer writes application code (preferred Mode A/B, see `GUIDE.md`).
- Local-only infrastructure: Docker Desktop cluster, Ollama and ChromaDB as in-cluster pods, no cloud LLM.
- Framework documents in English; project and personal documents in German.

## Open questions

- Which working mode to use for mini project step 1 (Mode A, B or C)?
- `AI_DEV_FRAMEWORK_README.md` now overlaps with the README framework section. Keep both, or remove one? (Developer decides; nothing deleted.)
- Are `CLAUDE.md` and `AGENTS.md` adapters wanted? They were created because `.claude/` and `.pi/` are present.
- Should `.scratch/` also be git-ignored by a more general pattern for other tooling? Current rule: `.scratch/` only.
