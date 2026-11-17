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

None in the application. Framework consolidation complete (CLAUDE.md → AGENTS.md, guideme note consolidated).

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
- Mini project step 1 uses Mode A (Independent Implementation).
- AGENTS.md is the hub; CLAUDE.md points to it and contains no content of its own.

## Open questions

- Should `.scratch/` also be git-ignored by a more general pattern for other tooling? Current rule: `.scratch/` only. It should be git-ignored in general.
