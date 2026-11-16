# k8s-doctor — Research Report

**Date:** 2026-11-16
**Scope:** Full folder inspection, all tracked files, git history, framework docs, project spec

---

## 1. What this project is

`k8s-doctor` is a **learning project** — not a commercial product, not a finished tool. Its purpose is threefold:

1. **CKAD exam prep** — every feature forces hands-on Kubernetes. No memorization, only building.
2. **Java re-entry** — Spring Boot backend, Fabric8 client, Spring AI.
3. **Modern AI architecture** — RAG, MCP, Agent Loop, all running fully local (no cloud LLM, no API keys).

It sits on Docker Desktop (3 nodes, WSL2 backend). All inference and vector storage happen as in-cluster pods.

---

## 2. Current state: empty repo with a blueprint

**No application code exists.** Zero Java, zero TypeScript, zero manifests. The repo is 100% scaffolding: framework documents, templates, and a detailed German-language spec (`brainstorm-001.md`).

Git log: 4 commits on `main`, pushed to `origin` (`github.com/Jangleur1991/k8s-doctor.git`).

```
107f4f2 ai dev framework init added
8ec001c ai dev framework prompt and readme added
ebc06e7 init
7a83cba Initial commit
```

Untracked: `.gitignore`, `.idea/`. No branches besides `main`.

---

## 3. Architecture (planned, not built)

```
[TypeScript CLI]  --HTTP (port-forward)-->  [Java Spring Boot Backend]  (Namespace: k8s-doctor)
   lokal                                        |-- Fabric8 → K8s-API (via ServiceAccount + RBAC)
                                                |-- Spring AI → Ollama-Pod (LLM)
                                                |-- Spring AI → ChromaDB-Pod (RAG)
                                                '-- MCP-Server (Tools: logs, events, patch)
```

| Component | Tech | Where | Purpose |
|---|---|---|---|
| CLI | TypeScript (strict) | Local machine, `tsx` or compiled | Terminal frontend: `diagnose`, `create`, `explain`, `agent start` |
| Backend | Java 25 + Spring Boot | In-cluster Deployment | Diagnose engine, agent loop, AI client, MCP server |
| K8s client | Fabric8 | Backend | Pods, events, logs, patches via Java |
| LLM | Ollama | In-cluster pod | Local inference. Model cached on PVC. |
| Vector store | ChromaDB | In-cluster StatefulSet + PVC | RAG over Kubernetes documentation |
| Agent loop | Custom | Backend | Watch → AI → Tools → Plan → User-OK → Apply → Verify |

---

## 4. Mini-project roadmap (steps 0–4)

The spec defines a concrete entry path (~30–60 min) that builds the foundation without AI:

| Step | What | CKAD topic |
|---|---|---|
| 0 | Create broken pod (`ImagePullBackOff`) in `playground` namespace | Troubleshooting |
| 1 | `GET /diagnose/{namespace}` — Spring Boot + Fabric8, JSON output | K8s API |
| 2 | `@Scheduled` watcher with deduplicated logging | Scheduling, state |
| 3 | Kubernetes manifests: namespace, RBAC, deployment, service | Deployment, RBAC, probes |
| 4 | `port-forward` + `curl` verification | Networking |

After mini-project: V1 (KI-Diagnose + CLI), V2 (RAG + MCP), V3 (Agent-Loop + Drill-Modus).

---

## 5. CKAD domain coverage plan

Every official CKAD domain maps to a concrete feature:

| Domain | Weight | Feature that covers it |
|---|---|---|
| Application Design and Build | 20% | Dockerfile (multi-stage), Workload types (Deployment/StatefulSet/CronJob/DaemonSet), Init containers, Volumes (PVC/emptyDir) |
| Application Deployment | 20% | Rolling updates, Blue/Green, Canary, Kustomize, Helm |
| Observability and Maintenance | 15% | Liveness/Readiness/Startup probes, `kubectl logs/describe/debug`, API deprecation checks |
| Environment, Configuration and Security | 25% | ConfigMaps, Secrets, ServiceAccount, RBAC, SecurityContext, ResourceQuota, LimitRange |
| Services and Networking | 20% | ClusterIP (internal DNS), NetworkPolicy, Ingress, NodePort |

Bonus: `k8s-doctor drill` — backend generates random broken pods; human fixes them on time; AI explains the solution.

---

## 6. AI Development Framework

This repo is the testbed for a **technology-agnostic, agent-independent AI collaboration framework**. Key documents:

| File | Purpose |
|---|---|
| `AI.md` | Universal entry point. Tells every AI agent which docs to read and when. Three concerns separated: AI learning, developer learning, project quality. |
| `GUIDE.md` | Collaboration protocol. Three working modes: A (independent), B (guided), C (AI-assisted). Learning as side effect. Architecture learning. Pattern learning. Communication rules. |
| `AI_RULES.md` | AI behaviour rules. Mistake → cause → prevention → detection → developer approval cycle. Empty rule registry so far. AI can never modify this file itself. |
| `RULES.md` | Project quality rules (enforceable/guideline/preference). Separate from AI behaviour. Also empty — rules added only after grounded in real code. |
| `TREE.md` | Repository map. Documents what exists, what doesn't yet. Distinguishes app code, tests, config, docs, framework files, scratch space, learning resources. |
| `PROGRESS.md` | Verified progress only. Current phase: Planning. No app code. Open question: which working mode for step 1? |
| `README.md` | Developer-facing quick start (German/English mix). |
| `AI_DEV_FRAMEWORK_README.md` | German quick-start version. Overlaps with README. Open: keep both or remove one. |
| `ai_dev_framework_prompt.md` | The original prompt that generated the framework. Reference only. 642 lines, very detailed. |

**Working modes:**

- **Mode A (Independent):** Developer codes first, AI creates optional reference in `.scratch/drafts/`, then both are compared and differences classified.
- **Mode B (Guided):** AI maps codebase, produces implementation guide. Developer writes the code. This is where the `guideme` skill applies.
- **Mode C (AI-assisted):** AI implements. Developer reviews and accepts.

**The `guideme` skill:** Present in both `.claude/skills/guideme/` and `.pi/agent/skills/guideme/`. Takes a request, maps the codebase slice, traces end-to-end impact, produces a step-by-step guide. Hard rule: does NOT create or edit production code. Reader is a senior engineer — no basic tutorials.

**Thin adapters:** `CLAUDE.md` and `AGENTS.md` both just say "read AI.md first, then GUIDE.md and AI_RULES.md." No duplicate rules.

---

## 7. File system structure

```
.
├── AI.md                     # Universal AI entry point
├── AGENTS.md                 # Thin adapter → AI.md
├── AI_DEV_FRAMEWORK_README.md # German quick-start (overlaps README)
├── AI_RULES.md               # AI behaviour rules (empty registry)
├── CLAUDE.md                 # Claude adapter → AI.md
├── GUIDE.md                  # Collaboration protocol, 3 modes
├── PROGRESS.md               # Verified progress (planning phase)
├── README.md                 # Project TODO + framework section
├── RULES.md                  # Project quality rules (empty registry)
├── TREE.md                   # Repository map
├── ai_dev_framework_prompt.md # Original framework prompt (642 lines)
├── brainstorm-001.md         # German project spec + roadmap
├── research.md               # (deleted/not present — referenced in TREE.md)
├── .gitignore                # Only .scratch/
├── .idea/                    # IntelliJ (untracked)
├── .claude/
│   ├── skills/
│   │   └── guideme/SKILL.md  # guideme skill for Claude Code
│   └── .cc-writes/           # Empty
├── .pi/
│   └── agent/skills/
│       └── guideme/SKILL.md  # Same skill for Pi agent
├── .scratch/                 # Git-ignored
│   ├── drafts/               # AI reference implementations (empty)
│   └── reviews/              # AI review artifacts (empty)
├── notes/
│   ├── README.md             # When/how to create notes
│   └── TEMPLATE.md           # What/Why/Example/Gotchas/Used in
├── patterns/
│   ├── README.md             # Active practice, not passive reading
│   └── TEMPLATE.md           # Problem/Canonical/Variations/Language/Trade-offs/Failure/When not/Used in
└── docs/
    └── decisions/
        └── TEMPLATE.md       # ADR format: Context/Decision/Alternatives/Consequences
```

Conventions:
- Framework docs in English (all agents can read them).
- Project/personal docs in German.
- `.scratch/` is the only git-ignored directory.
- No forced `src/` or `tests/` directories — structure follows the actual project when code appears.
- Templates are all technology-agnostic.

---

## 8. Specificities and observations

### 8.1 Research file missing
`TREE.md` references `research.md` as "Earlier research report on the project." It does not exist on disk. Either it was deleted after writing, or TREE.md is stale.

### 8.2 `AI_DEV_FRAMEWORK_README.md` overlap
The German quick-start overlaps significantly with the framework section of `README.md`. PROGRESS.md lists this as an open question: keep both or remove one. Neither is deleted.

### 8.3 Working mode undecided
PROGRESS.md has an open question: which mode (A, B, or C) to use for mini-project step 1. This affects how the AI participates when implementation starts.

### 8.4 No application code — yet
`TREE.md` explicitly states "None yet. The application is still at the planning stage." The spec (`brainstorm-001.md`) is detailed enough that implementation could start immediately. The barrier is the working-mode decision.

### 8.5 Git safety rules from parent repo
The parent directory (`Gorleben/`) has `.claude/rules/` with:
- `git-safety.md` — no destructive git commands without permission
- `go-conventions.md` — Go conventions for a different project ("streb")
- `security.md` — no sensitive file access, safe exec.Command, path safety
- `streb-architecture.md` — Provider interface, installer pattern, Cobra/Viper

These don't apply to k8s-doctor (it's Java + TypeScript, not Go), but the git safety rules are inherited.

### 8.6 Platform: WSL2 on Windows
Running via WSL2 (Linux 6.18.33.2-microsoft-standard-WSL2). Git repo is on the Windows filesystem (`/mnt/c/Users/...`). Docker Desktop runs on Windows, needs WSL2 backend integration for the cluster.

### 8.7 Dual agent support
The `guideme` skill is duplicated in both `.claude/` (Claude Code) and `.pi/` (Pi agent). The framework is designed to work across agent types — not Claude-only.

### 8.8 Local-only constraint
Explicit decision: no cloud LLM, no API keys. Everything runs as pods in the local Docker Desktop cluster. Ollama + ChromaDB deployed in-cluster. This is both a feature (privacy, offline) and a constraint (resource limits, no GPT-4 level models).

---

## 9. Summary

`k8s-doctor` is a **planning-stage learning project** wrapped in an **AI collaboration framework**. The framework (AI.md, GUIDE.md, AI_RULES.md, RULES.md, TREE.md, PROGRESS.md, notes/, patterns/, docs/decisions/) is mature and coherent. The application is empty but has a thorough spec (`brainstorm-001.md`) with a concrete 4-step entry path, CKAD domain mapping, and a clear version roadmap (mini-project → V1 → V2 → V3).

The repo is essentially a **well-architected empty space waiting for code** — the framework is the product right now, the k8s tool is the proving ground.
