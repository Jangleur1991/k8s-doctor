# k8s-doctor — Research Report

**Date:** 2026-11-16
**Repo:** `Jangleur1991/k8s-doctor`
**Branch:** `main` (3 commits, no application code yet)

---

## 1. What This Project Is

A **learning playground** — not a product, not an internal tool. The user (a German software developer) is building a Kubernetes diagnostics CLI+backend to achieve three simultaneous goals:

1. **CKAD exam prep** — every feature forces hands-on with an official CKAD domain
2. **Java re-entry** — get comfortable in Java/Spring Boot again
3. **AI engineering skills** — learn RAG, MCP, agent loops, vector stores

The project name `k8s-doctor` is literal: a CLI that diagnoses broken Kubernetes resources and explains how to fix them. But the *real* product is the developer's own skill growth.

---

## 2. Current State

**Empty shell.** No application code exists. The repo contains only:

| File | Purpose |
|---|---|
| `README.md` | TODO list (3 items) |
| `brainstorm-001.md` | Full project spec (German) — architecture, features, phases |
| `ai_dev_framework_prompt.md` | The prompt used to generate the AI dev framework (English) |
| `AI_DEV_FRAMEWORK_README.md` | Generated framework quick-start (German) |
| `.claude/skills/guideme/` | The `guideme` skill (analysis → implementation guide, not code) |
| `.pi/agent/skills/` | Pi agent's equivalent of guideme |

Git: 3 commits — initial, "init", "ai dev framework prompt and readme added". Remote on GitHub. No PRs, no issues configured.

**Verdict:** This is a planning/brainstorming repo, not a coding repo. The user is in the "decide what to build" phase, not the "build it" phase.

---

## 3. Architecture (As Designed)

```
[TypeScript CLI (local)]
        │ HTTP (port-forward)
        ▼
[Java Spring Boot Backend (in-cluster, namespace: k8s-doctor)]
    ├── Fabric8 Client → K8s API (pods, events, logs, patches)
    ├── Spring AI → Ollama Pod (LLM, local, no cloud API keys)
    ├── Spring AI → ChromaDB Pod (vector DB for RAG)
    └── MCP Server (tools: get_pod_logs, get_events, describe_resource, apply_patch)
```

Key design choices worth noting:

- **No cloud LLM.** Everything runs locally via Ollama in the cluster. No API keys, no cost.
- **Java backend, TS CLI.** Split responsibility: Java does the heavy lifting (K8s API, AI, MCP), TypeScript handles terminal UX.
- **ChromaDB for RAG.** K8s documentation vectorized → retrieval-augmented prompts for YAML generation and concept explanation.
- **Agent Loop (V3).** Not just a chat bot — a scheduled watcher that sees problems, reasons about them, proposes fixes, and *only applies after user approval*.

---

## 4. Feature Roadmap (4 Phases)

### Phase 0 — Mini Project (30-60 min)
The bootstrap path. No AI, no CLI. Just:
1. Create a broken pod (`ImagePullBackOff`)
2. Build `GET /diagnose/{namespace}` endpoint (Java/Spring Boot + Fabric8)
3. Add `@Scheduled` watcher that logs new problems (deduped)
4. Deploy to cluster with proper RBAC (ServiceAccount, ClusterRole, ClusterRoleBinding)
5. Expose via `port-forward`

**CKAD covered:** Deployments, Services, Probes, Requests/Limits, ServiceAccount, RBAC, port-forward, logs, troubleshooting.

### Phase 1 — KI Diagnose + CLI
- Ollama deployment (with PVC for model cache)
- Spring AI ChatClient sends diagnosis JSON → gets structured response (cause, fix, YAML patch)
- TypeScript CLI (`commander`) displays table + AI explanation

### Phase 2 — RAG + Create + MCP
- K8s docs indexed into ChromaDB (StatefulSet + PVC)
- `k8s-doctor create cronjob --schedule "*/5 * * * *"` → valid YAML output
- MCP server exposes K8s tools for AI agent use

### Phase 3 — Agent Loop + Drill
- Watcher evolves into autonomous agent: see → reason → plan → ask → apply → verify
- `k8s-doctor drill` — backend randomly breaks pods, user fixes them on timer, AI explains solution

---

## 5. CKAD Coverage Matrix

Every CKAD domain maps to a concrete project task:

| Domain (Weight) | Project Task |
|---|---|
| **App Design & Build (20%)** | Dockerfile (multi-stage Java), Deployment/StatefulSet/CronJob/DaemonSet, init containers, sidecars, PVCs, emptyDir |
| **App Deployment (20%)** | Rolling updates, rollout status/undo, Blue/Green or Canary, Kustomize (base + overlays), Helm charts |
| **Observability (15%)** | Liveness/Readiness/Startup probes, logs/describe/events/top, exec/debug, API deprecation checks |
| **Config & Security (25%)** | ConfigMaps/Secrets, ServiceAccount/Roles, SecurityContext (runAsNonRoot, readOnlyRootFilesystem), Requests/Limits, ResourceQuota/LimitRange, optional CRD |
| **Networking (20%)** | ClusterIP DNS, NetworkPolicy, Ingress vs NodePort, k8s-doctor as the networking lab |

---

## 6. AI Development Framework

This repo also holds a **technology-agnostic AI collaboration framework**, generated from `ai_dev_framework_prompt.md`. The framework defines how developer and AI work together:

### Three Working Modes

| Mode | When |
|---|---|
| **Independent Implementation** | Developer codes first, AI creates reference draft in `.scratch/drafts/` for comparison afterward |
| **Guided Implementation** | AI analyzes codebase, produces implementation guide (uses `guideme` skill), developer codes |
| **AI-Assisted Implementation** | AI implements, developer reviews and accepts |

### Framework Artifacts (Planned, Not Yet Created)

The framework spec defines these files/dirs but they **do not exist in the repo yet**:
- `AI.md` — universal AI entry point
- `GUIDE.md` — collaboration protocol
- `AI_RULES.md` — AI behavior rules (learned from mistakes)
- `RULES.md` — project quality rules
- `TREE.md` — repository map
- `PROGRESS.md` — project state tracker
- `.scratch/drafts/` and `.scratch/reviews/` — AI artifacts
- `notes/` — developer knowledge capture
- `patterns/` — pattern learning
- `docs/decisions/` — ADRs

Only `AI_DEV_FRAMEWORK_README.md` (the quick-start guide) was generated. The rest are defined but uncreated.

### Core Principles
- Developer owns all decisions
- AI never autonomously changes `AI_RULES.md`
- Mistakes get analyzed → rules added (after developer approval)
- Reference implementations never auto-deployed to production
- Framework adapts to project, not vice versa

---

## 7. Specificities & Notable Things

### Non-Standard Choices
1. **Java 25** (not 17/21). Bleeding edge. Spring Boot on it means cutting-edge Spring too.
2. **Fabric8 client** (not the official Kubernetes client Java). More Spring-friendly, but less actively maintained.
3. **Docker Desktop cluster** (not minikube/kind/k3s). Limited resources, but familiar.
4. **German documentation** (`README.md`, framework docs) alongside English specs. Bilingual repo.

### What's Different About This Repo
- **It's a learning journal, not a product.** The code is the medium; the skills are the goal.
- **The AI framework is a meta-project.** It's about *how to work with AI*, sitting alongside the actual `k8s-doctor` project.
- **Phase-gated by enjoyment.** "Ghostty-Modus" — if mini-project is fun, continue. If not, stop. No sunk-cost fallacy.
- **No CI/CD, no tests, no build system.** Pre-code. The first actual file to be created hasn't been written yet.

### Constraints
- Local cluster only (Docker Desktop, 3 nodes)
- No cloud APIs, no API keys
- Ollama + ChromaDB run as pods (not local installs)
- TypeScript CLI must use `strict: true`, no hand-written JS

---

## 8. Current Blockers / Next Steps

From the README TODO:
1. "Try out the guideme skill" — hasn't happened yet
2. "Read the brainstorm plan" — this report covers it
3. "Establish AI-driven development" — framework docs mostly defined but not all files created

**Immediate next step (per brainstorm-001):** Mini-Projekt Schritt 0–4:
1. Create `playground` namespace + broken pod
2. Scaffold Spring Boot project (`start.spring.io` + Web, Actuator + Fabric8)
3. Implement `GET /diagnose/{namespace}` endpoint
4. Deploy with RBAC
5. Verify via `port-forward` + `curl`

---

## 9. Files in Repo (Complete Inventory)

```
k8s-doctor/
├── .claude/
│   └── skills/
│       └── guideme/
│           └── SKILL.md              ← analysis → guide (no code)
├── .pi/
│   └── agent/
│       └── skills/
│           └── guideme/
│               └── SKILL.md          ← copy for Pi agent
├── .idea/                            ← IntelliJ (untracked)
├── .git/
├── README.md                         ← TODO (3 items)
├── brainstorm-001.md                 ← full project spec (German)
├── ai_dev_framework_prompt.md        ← framework generation prompt (English)
└── AI_DEV_FRAMEWORK_README.md        ← framework quick-start (German)
```

**Application code: 0 files.** Framework docs: 4 files (partially implemented).
