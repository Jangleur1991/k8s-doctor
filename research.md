# k8s-doctor — Research Report

**Date:** 2026-11-17
**Scope:** Full folder inspection — all tracked files, git history, framework documentation, project specification, and structural analysis

---

## 1. What This Project Is

`k8s-doctor` is a **deliberate learning project**, not a commercial product or a finished CLI tool. It sits at the intersection of three learning goals:

1. **CKAD certification prep through building** — every feature forces hands-on Kubernetes. There is no memorization without implementation.
2. **Java re-entry** — Spring Boot backend with Fabric8 Kubernetes Client, targeting Java 25.
3. **Modern AI architecture** — RAG, MCP, and a custom Agent Loop, all running **fully local** (Docker Desktop, Ollama, ChromaDB as in-cluster pods — no cloud LLM, no API keys).

The project is structured as a **split architecture**: a TypeScript CLI (local) speaks HTTP to a Java Spring Boot backend (running in-cluster). The backend does the heavy lifting: K8s API interaction via Fabric8, LLM inference via Spring AI → Ollama, RAG via Spring AI → ChromaDB, and an MCP server exposing tools.

---

## 2. Current State: Scaffolding + Blueprint

### 2.1 Application Status

**The application itself is still at the planning stage.** The `backend/` directory contains a minimal Spring Boot 4.1.2 skeleton (Java 21, `gradle/wrapper/gradle-wrapper.jar` at Gradle 9.9.1) with:

- A single `@SpringBootApplication` class with a `main` method and nothing else.
- Empty test class (`contextLoads()` — JUnit 5, Spring Boot Test).
- `application.properties` with only `spring.application.name=k8sdoctor`.
- Dependencies declared but not yet extended: currently just `spring-boot-starter-actuator` and `spring-boot-starter-webmvc` (no Fabric8, no Spring AI).

No TypeScript CLI code exists. No Kubernetes manifests exist. No Dockerfile exists. Nothing in `k8s/`.

### 2.2 Git History

5 commits on `main`, pushed to `origin` (`github.com/Jangleur1991/k8s-doctor.git`):

| Commit | Message |
|---|---|
| `25f38e5` | init backend, gradle notes added |
| `364b515` | research.md added |
| `107f4f2` | ai dev framework init added |
| `8ec001c` | ai dev framework prompt and readme added |
| `ebc06e7` | init |
| `7a83cba` | Initial commit |

The working tree is currently dirty: a vim swap file `backend/.build.gradle.swp` has been deleted and `research.md` (an earlier version) has been deleted (the file we are now creating replaces it). Untracked files: `.gitignore` (AI framework scratch rule), `.idea/` (IntelliJ project files).

### 2.3 Working Branch

No branches other than `main`. The project is early-stage; no feature branches yet.

---

## 3. The AI Development Framework — A Meta-Layer

The most unusual and intentional aspect of this project is the **AI Development Framework** — a sophisticated, self-improving layer of process documentation that sits on top of the application code and governs how AI agents and the developer collaborate.

### 3.1 Architecture of the Framework

The framework uses a **hub-and-spoke model** with `AI.md` as the universal, agent-independent entry point. Every AI agent reads it first:

```
AI.md (universal entry point)
  ├── GUIDE.md (collaboration & learning protocol)
  ├── AI_RULES.md (AI behaviour rules — developer-approved only)
  ├── RULES.md (project quality rules — technology-agnostic)
  ├── TREE.md (repository map)
  └── PROGRESS.md (verified project state)
```

Additional documents feed in on demand: `notes/`, `patterns/`, `docs/decisions/`.

### 3.2 Three Separate Concerns — Enforced Separation

The framework enforces strict separation of three concerns:

| Concern | Document | Owner |
|---|---|---|
| Improving AI behaviour | `AI_RULES.md` | Developer (AI proposes only) |
| Helping developer learn | `GUIDE.md` + `notes/` | Developer |
| Maintaining project quality | `RULES.md` | Developer |

This is a deliberate design to prevent the common AI mistake of conflating "how I should behave" with "what the project requires."

### 3.3 Three Working Modes

The developer picks a mode per task:

| Mode | AI Role | Developer Role | When |
|---|---|---|---|
| **A — Independent** | Optional reference in `.scratch/drafts/`, comparison after | Writes production code first | Learning preferred |
| **B — Guided** | Investigates real code, produces implementation guide, explains context | Writes the code | Understanding existing codebase |
| **C — AI-assisted** | Implements, verifies, explains decisions | Reviews and accepts | Explicit request |

The developer can switch modes at any time. Mode A is the preferred default for learning.

### 3.4 AI Mistake Learning Loop

When the AI makes a mistake, a structured 7-step process triggers:

1. Describe the actual mistake
2. Determine its cause (missing context, unclear instructions, incorrect assumption, ignored constraint, etc.)
3. Propose a preventive rule
4. Decide whether automated detection is possible
5. Ask the developer to approve the proposal
6. Only after approval, add the rule
7. Add automated checks only when authorised and technically appropriate

**Key constraint:** No AI agent may autonomously modify `AI_RULES.md`. The developer approves every rule. The rule registry in `AI_RULES.md` is currently **empty** — no mistakes have been observed yet (project is pre-code).

### 3.5 Rule Formats

**AI Rules** (`AI_RULES.md`):
```
### AIR-NNN — Name
- Observed problem:
- Required behaviour:
- Prevention:
- Detection: (method, or "none yet")
- Status: proposed | approved | retired
- Approved by / date:
```

**Project Rules** (`RULES.md`):
```
### RUL-NNN — Name  [enforceable | guideline | preference]
- What must be true:
- Why it matters:
- Detection method:
- Check location: (file/command, or "none yet")
- Status: proposed | active | retired
```

Both registries are empty. The framework explicitly says: "Do not invent technology-specific rules ahead of the code."

### 3.6 Agent-Specific Thin Adapters

Agent-specific files exist only where the environment requires them:

| File | Purpose | Contains |
|---|---|---|
| `CLAUDE.md` | Claude Code adapter | One pointer to `AI.md` + one note about the `guideme` skill |
| `AGENTS.md` | Generic agent adapter | One pointer to `AI.md` + "Do not add rules here" |

Both are explicitly documented as "thin adapters." The framework philosophy: `AI.md` is the source of truth. Don't copy rules into agent-specific files.

### 3.7 The `guideme` Skill

A specialized skill is present for two agents:

- `.claude/skills/guideme/SKILL.md` (Claude Code)
- `.pi/agent/skills/guideme/SKILL.md` (Pi agent)

Both files are **identical**. The `guideme` skill produces **implementation guides, not implementations**. It treats the developer as a senior engineer being onboarded to the codebase. Key rules:

- Must NOT create, edit, or write production code, config, or tests.
- Must read the actual codebase before writing the guide — never guess.
- The reader is a senior engineer — no explaining language syntax or basics.
- Points to specific anchors: `path/to/file.ext → functionOrBlock` (line ranges if stable).
- When something is ambiguous, surfaces it as a decision for the human.

The skill follows a 6-step checklist: restate request, map codebase slice, trace end-to-end, draft guide steps, define verification, list open decisions.

---

## 4. Documentation Files — Purpose and Cross-References

### 4.1 Project-Specification Files (German)

| File | Purpose | Status |
|---|---|---|
| `brainstorm-001.md` | **Master project specification** — tech stack, architecture, mini-project roadmap, CKAD coverage plan, V1→V3 evolution, bonus features | Authoritative spec |
| `AI_DEV_FRAMEWORK_README.md` | Earlier German quick-start for the framework | **Overlaps** with README framework section — open question in PROGRESS.md |

### 4.2 Framework Files (English)

| File | Purpose | Length (approx.) |
|---|---|---|
| `AI.md` | Universal entry point — "read these files, in this order" | 2484 bytes |
| `GUIDE.md` | Collaboration protocol, working modes, learning as side effect, pattern learning, ADR process | 3901 bytes |
| `AI_RULES.md` | AI behaviour rules — mistake learning loop, rule format, empty registry | 1585 bytes |
| `RULES.md` | Project quality rules — enforceable/guideline/preference categories, empty registry | 923 bytes |
| `TREE.md` | Repository map — what's where, what's tracked, conventions | 2835 bytes |
| `PROGRESS.md` | Verified progress only — no planned work marked done, current task, open questions | 2110 bytes |
| `README.md` | Hybrid: project TODO list + short framework section | 1024 bytes |

### 4.3 Prompt & Reference

| File | Purpose |
|---|---|
| `ai_dev_framework_prompt.md` | The exact prompt that generated the framework — included as reference for reproducing or understanding the original design intent |

### 4.4 Template System

Three template ecosystems, all technology-agnostic and empty of content:

- **Notes** (`notes/README.md`, `notes/TEMPLATE.md`): Practical knowledge captured during development. Fields: What, Why, Example, Gotchas, Used in.
- **Patterns** (`patterns/README.md`, `patterns/TEMPLATE.md`): Active pattern practice separate from production code. Fields: Problem solved, Canonical structure, Variations, Language comparison, Trade-offs, Failure cases, When not to use, Used in.
- **ADR** (`docs/decisions/TEMPLATE.md`): Lightweight Architecture Decision Records. Fields: Context, Decision, Alternatives considered, Consequences. Status: proposed/accepted/superseded.

### 4.5 Existing Notes (6 files)

**Three original notes** (all about Gradle) — originally written with placeholder text in the What/Why/Example sections:

1. `gradle-files.md` — Gradle folder structure, the three essential files (settings.gradle, build.gradle, gradlew)
2. `gradle-plugins.md` — How plugins extend Gradle (java, spring-boot, dependency-management), contrast with Maven parent POM
3. `gradle-toolchain.md` — How toolchain configuration lets Gradle use a different JDK version than the build JDK

**Three rewritten notes** (also about Gradle) — fully filled out in English with real examples, gotchas, and Used-in references:

4. `gradle-jar-vs-bootjar.md` — Standard JAR vs Spring Boot fat JAR, manifest differences, deployability
5. `gradle-tasks.md` — Tasks as atomic work units, task DAGs, lazy execution, UP-TO-DATE tracking
6. `gradle-configuration-cache.md` — Configuration Cache for faster repeated builds, invalidation rules, caveats

All six follow the TEMPLATE.md format. Notably:

- The original three were written in **German**; the rewritten three are in **English**.
- All notes contain actual implementation references (e.g., `backend/build.gradle`).
- The "Gotchas" sections in the rewritten notes are fully populated — a sign the developer engaged deeply with the concepts.

---

## 5. Backend Codebase — Current State

### 5.1 Build System

- **Gradle 9.9.1** (via wrapper — `gradlew`, `gradlew.bat`, `gradle/wrapper/gradle-wrapper.jar`)
- Spring Boot 4.1.2 (very new — this is a cutting-edge version)
- **Java 21** toolchain (changed from Java 25 — no JDK 25 available on the build machine; JDK 21 is sufficient for Spring Boot with Fabric8)
- Group: `com.example`, project name: `k8sdoctor`, version: `0.0.1-SNAPSHOT`
- `settings.gradle`: simple single-project definition

### 5.2 Dependencies (Declared, Not Used Yet)

Only Spring Boot starters — no Kubernetes client, no AI libraries:

| Dependency | Purpose |
|---|---|
| `spring-boot-starter-actuator` | Health endpoints (`/actuator/health/...`) for liveness/readiness probes |
| `spring-boot-starter-webmvc` | REST endpoints |

**Missing but needed per spec** (not yet added):
- `io.fabric8:kubernetes-client` — K8s API access
- Spring AI dependencies — Ollama integration, RAG, MCP
- Spring AI starters — Vector store, ChatClient, @Tool annotations

### 5.3 Application Code

**Zero application logic.** The `K8sdoctorApplication.java` is a pure scaffold — just `@SpringBootApplication` and `main()`. Same for tests: one `contextLoads()` test.

This is consistent with the project being in "Planning" phase per `PROGRESS.md`.

### 5.4 Configuration

`application.properties` has only:
```properties
spring.application.name=k8sdoctor
```
Nothing for K8s config resolution, actuator endpoints, or any other configuration.

---

## 6. The Project Specification (`brainstorm-001.md`)

This is the most detailed file in the project — a thorough, well-structured spec written in German.

### 6.1 Tech Stack

| Component | Technology | Role |
|---|---|---|
| Backend | Java 25 + Spring Boot | Diagnose engine, Agent Loop, KI-Client, MCP-Server (~60% of work) |
| K8s Client | Fabric8 | Pods, Events, Logs, Patches via Java |
| AI Framework | Spring AI | ChatClient → Ollama, VectorStore for RAG, @Tool annotations, MCP server starter |
| CLI | TypeScript (strict: true) | Terminal frontend, `tsx` or compiled |
| LLM | Ollama | Local LLM as in-cluster pod |
| Vector DB | ChromaDB | RAG over K8s docs |
| Infra | Docker Desktop | 3-node WSL2 cluster |

### 6.2 Four CLI Command Groups

| Command | Function |
|---|---|
| `k8s-doctor diagnose [deploy]` | Collects pod status, events, logs → AI explains cause + suggests fix |
| `k8s-doctor create <resource>` | AI generates valid YAML (RAG over K8s docs) → developer reviews → apply |
| `k8s-doctor explain <thema>` | AI explains K8s concept with example from current cluster |
| `k8s-doctor agent start` | Agent Loop monitors cluster, reports problems, suggests fixes (apply only with user OK) |

### 6.3 Mini-Project Roadmap (Steps 0–4)

A concrete entry path (~30–60 minutes) to validate the foundation without any AI:

| Step | What | CKAD Topic |
|---|---|---|
| 0 | Create broken pod (`ImagePullBackOff`) in `playground` namespace | Troubleshooting |
| 1 | `GET /diagnose/{namespace}` — Spring Boot + Fabric8, returns JSON | K8s API |
| 2 | `@Scheduled` watcher with deduplicated logging | Scheduling, state management |
| 3 | K8s manifests: namespace, RBAC, deployment, service | Deployment, RBAC, probes |
| 4 | `port-forward` + `curl` verification | Networking |

After mini-project: rolling upgrades to V1 (KI-Diagnose + CLI), V2 (RAG + MCP), V3 (Agent-Loop + Drill-Modus).

### 6.4 CKAD Domain Coverage

Every official CKAD domain maps to a concrete implementation:

| Domain | Weight | Feature |
|---|---|---|
| Application Design and Build | 20% | Dockerfile (multi-stage), Workload types, Init containers, Volumes |
| Application Deployment | 20% | Rolling updates, Blue/Green, Canary, Kustomize, Helm |
| Observability and Maintenance | 15% | Probes, logs/describe/debug, API deprecation checks |
| Environment, Configuration and Security | 25% | ConfigMaps, Secrets, ServiceAccount, RBAC, SecurityContext, ResourceQuota, LimitRange |
| Services and Networking | 20% | ClusterIP DNS, NetworkPolicy, Ingress, NodePort |

**Bonus feature:** `k8s-doctor drill` — backend generates random broken pods (wrong image, wrong probe port, missing secret, insufficient memory, blocking NetworkPolicy); developer fixes them on time; AI explains the solution afterward. Like killer.sh but in your own cluster.

---

## 7. Directory Structure

```
k8s-doctor/
├── .claude/                          # Agent config (untracked)
│   └── skills/guideme/SKILL.md       # Implementation guide skill
├── .git/                             # Version control
├── .idea/                            # IntelliJ IDEA (untracked)
├── .pi/                              # Pi agent config
│   └── agent/skills/guideme/SKILL.md # Same skill for Pi
├── .scratch/                         # AI drafts & reviews (git-ignored)
│   ├── drafts/                       # Empty — reference implementations
│   └── reviews/                      # Empty — AI review artifacts
├── docs/
│   └── decisions/
│       └── TEMPLATE.md               # ADR template
├── backend/                          # Spring Boot skeleton (no app logic yet)
│   ├── .gitattributes
│   ├── .gitignore
│   ├── .gradle/                      # Gradle cache (build artifacts)
│   ├── HELP.md
│   ├── build.gradle                  # Spring Boot 4.1.2, Java 21, Gradle 9.9.1
│   ├── gradlew / gradlew.bat         # Gradle wrapper
│   ├── gradle/wrapper/
│   │   ├── gradle-wrapper.jar
│   │   └── gradle-wrapper.properties
│   ├── settings.gradle               # project name: k8sdoctor
│   └── src/
│       ├── main/
│       │   ├── java/com/example/k8sdoctor/
│       │   │   └── K8sdoctorApplication.java  # Empty scaffold
│       │   └── resources/
│       │       └── application.properties     # spring.application.name only
│       └── test/java/com/example/k8sdoctor/
│           └── K8sdoctorApplicationTests.java # contextLoads()
├── notes/                            # Developer learning notes
│   ├── README.md                     # When/why/how to write notes
│   ├── TEMPLATE.md                   # Note template
│   ├── gradle-files.md               # Gradle file structure (German)
│   ├── gradle-plugins.md             # Gradle plugins (German)
│   └── gradle-toolchain.md           # Gradle toolchain (German)
├── patterns/                         # Pattern practice material
│   ├── README.md                     # How to learn patterns actively
│   └── TEMPLATE.md                   # Pattern template
├── AGENTS.md                         # Thin adapter → AI.md
├── AI.md                             # Universal AI entry point
├── AI_DEV_FRAMEWORK_README.md        # German framework quick-start (overlaps)
├── AI_RULES.md                       # AI behaviour rules (empty registry)
├── CLAUDE.md                         # Thin adapter → AI.md
├── GUIDE.md                          # Collaboration protocol
├── PROGRESS.md                       # Project progress (Planning phase)
├── README.md                         # Hybrid: TODO + framework section
├── RULES.md                          # Project quality rules (empty registry)
├── TREE.md                           # Repository map
├── ai_dev_framework_prompt.md        # Original framework generation prompt
├── brainstorm-001.md                 # Master project specification (German)
├── research.md                       # THIS file (new research report)
└── .gitignore                        # .scratch/ rule only
```

---

## 8. Key Observations and Design Decisions

### 8.1 Intentional Deliberateness

This project is **highly intentional** about its own process. The AI Development Framework is not boilerplate — it reflects a mature understanding of how AI agents fail and how to build guardrails. Key signals:

- **Empty rule registries everywhere** — the framework explicitly says "do not invent rules ahead of code." This is a guardrail against over-engineering process before understanding the code.
- **Separation of concerns enforced at the document level** — AI behaviour rules vs. project quality rules vs. learning protocol are three separate files with separate audiences.
- **Agent independence** — `AI.md` does not assume Claude, VS Code, or any provider. The thin adapter pattern (`CLAUDE.md`, `AGENTS.md`) is a clean way to support multiple agents without duplication.
- **Developer in the loop** — the preferred mode (A) requires the developer to write code first, then optionally compare with an AI draft. This prevents AI from becoming a crutch.

### 8.2 Language Strategy

The project documents live in **two languages**:

- **English** for framework files (`AI.md`, `GUIDE.md`, `AI_RULES.md`, `RULES.md`, `TREE.md`, `PROGRESS.md`, templates) — so every AI agent, regardless of locale, can read them.
- **German** for project content (`README.md`, `brainstorm-001.md`, `AI_DEV_FRAMEWORK_README.md`, and all developer-written notes) — because the developer's thinking language is German.

This bilingual split is a deliberate design documented in `TREE.md` under "Conventions."

### 8.3 The Learning-Centric Architecture

The framework treats learning as a first-class concern:

- **Learning as a side effect** (GUIDE.md §5.2): knowledge captured during work, not retrofitted.
- **Pattern practice** (GUIDE.md §5.4): explicitly separated from production code — practicing a pattern never auto-introduces it into the application.
- **Architecture learning** (GUIDE.md §5.3): compare alternatives with fixed requirements, explaining trade-offs and failure modes.

### 8.4 Infrastructure Constraints

- **Local-only** — Docker Desktop cluster, Ollama pod, ChromaDB pod. No cloud LLM, no API keys.
- **3 nodes** on Docker Desktop (WSL2 backend).
- **No Kubernetes manifests yet** — the spec says "self-write (Doku erlaubt, nicht kopieren)" — documentation is allowed for reference, but code must be written by hand for learning value.

### 8.5 The Spec's Sophistication

`brainstorm-001.md` is not a throwaway idea document. It's a thorough specification that:

- Maps every CKAD domain to a concrete implementation task.
- Defines a realistic mini-project (~30-60 min) to validate the foundation.
- Plans a V1→V3 evolution path.
- Includes a bonus "drill mode" concept.
- Addresses networking (ClusterIP DNS, NetworkPolicy, Ingress), security (RBAC, SecurityContext), observability (probes, logs, debug), and deployment (rolling updates, blue/green, Kustomize, Helm).

### 8.6 What's Not There (Yet)

- No actual Kubernetes manifests (`k8s/` directory doesn't exist)
- No Dockerfile
- No TypeScript project
- No application logic beyond the Spring Boot scaffold
- No CI/CD pipeline
- No tests (beyond `contextLoads()`)
- No actual dependencies for K8s client or Spring AI
- No ADRs recorded
- No patterns practiced
- No rules in AI_RULES.md or RULES.md registries

This is consistent with being in "Planning" phase. The framework is ready; the code is not.

---

## 9. Open Questions (From PROGRESS.md)

Three open questions are explicitly tracked:

1. **Working mode for mini project step 1** — Mode A (independent), B (guided), or C (AI-assisted)? Decision deferred to the developer.
2. **Document overlap** — `AI_DEV_FRAMEWORK_README.md` overlaps with the README framework section. Keep both or remove one? (Nothing deleted yet — developer decides.)
3. **Agent adapters** — `CLAUDE.md` and `AGENTS.md` were created because `.claude/` and `.pi/` directories exist. Are they wanted?

---

## 10. Architectural Assessment

### 10.1 Strengths

- **The AI Development Framework is genuinely novel.** Most projects that use AI agents have no process for learning from AI mistakes or for separating AI behaviour from project quality. This project does both explicitly.
- **The three-mode working system** (independent/guided/AI-assisted) gives the developer real choice over AI involvement per task.
- **The `guideme` skill** is well-designed — it produces implementation guides, not code, treating the developer as a senior engineer.
- **The mini-project roadmap** is concrete and time-boxed. Steps 0-4 can be completed in 30-60 minutes and immediately validate the backend-in-cluster approach.
- **The bilingual document strategy** is practical — English for AI-agnostic process, German for developer thinking.
- **The empty-registry discipline** prevents premature over-engineering. No rules are added until they're grounded in real experience.

### 10.2 Risks and Considerations

- **Gradle 9.9.1 + Spring Boot 4.1.2 + Java 21** — all extremely new (Java version downgraded from 25 to 21 after JDK 25 was unavailable on the build machine). There may be compatibility quirks or limited ecosystem support for Spring Boot 4.1.2 and Gradle 9.9.1.
- **No existing code to anchor rules in** — AI_RULES.md and RULES.md are empty by design, but this means there are zero guardrails for the first AI interaction. The first mistake will define the first rule.
- **Spec complexity vs. initial scope** — `brainstorm-001.md` describes a very large system (RAG + MCP + Agent Loop + Drill Mode + full CKAD coverage). The mini-project is small, but the gap between "first endpoint works" and "full system" is substantial.
- **Docker Desktop cluster networking** — running 3 nodes on Docker Desktop via WSL2 can have networking quirks, especially for in-cluster pod-to-pod communication (Ollama, ChromaDB → backend).

### 10.3 Process Quality

The project's process documentation is exceptional for a personal learning project. The framework reflects:

- Understanding of how AI agents fail (inventing APIs, ignoring architecture, editing out-of-scope files)
- A structured mistake-learning process
- Clear separation between AI behaviour, developer learning, and project quality
- Agent independence (no vendor lock-in for the process)
- Developer-in-the-loop design

This is more process discipline than many enterprise projects have.

---

## 11. Summary

`k8s-doctor` is a **learning project with a process-first mindset**. The application is a planned split architecture (TypeScript CLI → Java Spring Boot backend → K8s API + Ollama + ChromaDB) for diagnosing Kubernetes problems with local AI. Nothing has been built yet.

What *has* been built is a **sophisticated AI Development Framework** that governs how AI agents and the developer collaborate, learn from mistakes, and maintain quality. This framework is technology-agnostic, agent-independent, and designed to be reusable across repositories.

The project is in **Planning** phase. The next step — per the mini-project roadmap — is to create a broken pod, build the `/diagnose` endpoint, deploy it with RBAC, and verify via `port-forward`. The working mode for this step (A, B, or C) is still an open question.

The project's most distinctive feature is not its architecture or its tech stack — it's the **explicit, structured approach to AI collaboration** that sits on top of everything else.
