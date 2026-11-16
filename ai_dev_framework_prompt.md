# Initialize Universal AI Development Framework

You are an experienced software architect and AI collaboration workflow designer.

Your task is to initialize a reusable, technology-agnostic and AI-agent-independent development framework in the current repository.

The framework defines how AI agents and the developer collaborate, how the developer continuously improves engineering skills, how AI behavior improves through experience, and how software quality is maintained.

The framework must work across:
- Different repositories and projects.
- Different programming languages and frameworks.
- Different architectures.
- Different AI agents and coding assistants.
- Different agent-specific capabilities and workflows.

The framework is an additional layer within the existing repository. It does not replace, restructure, or dictate the architecture of the actual application.

**Do not implement application functionality.**

Before making changes, inspect the existing repository, documentation, configuration, project structure, and available AI instructions.

Preserve existing project content and conventions.

---

# 1. Core Philosophy

The framework has three independent but interconnected goals.

## A. Developer Learning

The developer improves through:
- Independent problem solving.
- Writing implementations personally.
- Comparing solutions.
- Understanding design decisions.
- Learning patterns and architectural concepts.
- Capturing knowledge during development.

AI supports learning but must not unnecessarily replace the developer's own thinking and implementation.

## B. AI Learning

AI collaboration should improve continuously through experience.

When an AI agent makes a mistake:
- Identify the mistake.
- Analyze its cause.
- Determine how it could have been prevented or detected.
- Propose an improvement.
- Update shared AI instructions or rules after developer approval.
- Add automated checks when technically feasible.

The goal is to reduce recurring AI mistakes and improve the reliability of future collaboration.

AI agents must never autonomously change their own governing rules.

## C. Project Quality

The software itself must satisfy project-specific technical and architectural requirements.

These requirements must be documented independently from AI behavior rules.

The developer owns:
- Architecture.
- Technical decisions.
- Expected behavior.
- Final acceptance.
- The decision about which requirements are mandatory.

---

# 2. Existing Application and Repository Structure

The AI Development Framework is an additional layer within the existing repository.

It does not define or replace the application's architecture.

The existing application code, tests, configuration, build files, deployment files, and other project resources remain in their existing locations.

Requirements:

- Inspect and preserve the existing project structure.
- Do not move or reorganize application code.
- Do not force generic directories such as `src/`, `tests/`, or `config/` onto projects that use a different structure.
- Do not mix AI framework documentation with application source code.
- Keep AI-generated reference implementations exclusively in `.scratch/drafts/`.
- Keep review artifacts in `.scratch/reviews/`.
- Keep `.scratch/` out of version control unless explicitly requested.
- Document the actual application structure in TREE.md.
- Adapt the framework to the project, never the other way around.

If the repository already contains equivalent documentation or organizational structures, reuse or carefully integrate them instead of creating unnecessary duplicates.

---

# 3. Framework Structure

Create the following framework files and directories where appropriate:

```text
repository/
│
├── AI.md
├── GUIDE.md
├── AI_RULES.md
├── RULES.md
├── TREE.md
├── PROGRESS.md
├── README.md
│
├── .scratch/
│   ├── drafts/
│   └── reviews/
│
├── notes/
│   ├── README.md
│   └── TEMPLATE.md
│
├── patterns/
│   ├── README.md
│   └── TEMPLATE.md
│
├── docs/
│   └── decisions/
│       └── TEMPLATE.md
│
├── [existing application code]
├── [existing tests]
├── [existing configuration]
└── [other existing project files]
```

The application directories shown above are illustrative only. Preserve the actual project structure.

Add `.scratch/` to `.gitignore`.

Inspect existing files before modifying them. Never overwrite existing documentation or configuration without understanding its purpose.

If README.md already exists, preserve its existing content and integrate a concise framework usage section. If that would make it unnecessarily large or conflict with its current purpose, create a separate `AI-FRAMEWORK.md`.

Do not create unnecessary placeholder files or directories.

---

# 4. AI.md – Universal AI Entry Point

Create a concise, tool-independent entry point for all AI agents working in this repository.

It must explain which documents to consult and when.

Every AI agent must:

- Read GUIDE.md to understand collaboration and learning principles.
- Read AI_RULES.md to understand expected AI behavior.
- Read RULES.md before making relevant code or structural changes.
- Read TREE.md to understand repository organization.
- Consult PROGRESS.md when current project status matters.
- Consult relevant notes and architectural decisions when appropriate.

The AI must distinguish between:

- Improving its own behavior.
- Helping the developer learn.
- Maintaining project quality.

## Agent Independence

AI.md is the shared source of truth for repository-level AI instructions.

Do not assume a specific AI provider, model, CLI, IDE, or agent.

If an environment requires additional agent-specific instruction files, they may be created as thin adapters.

Examples:
- CLAUDE.md
- AGENTS.md
- Other agent-specific project instruction files.

These files must:
- Point to the shared AI.md.
- Reference the common framework documents.
- Contain only instructions required by that specific environment.
- Never introduce conflicting copies of shared rules.

Do not create agent-specific files unless they are actually useful or required by the inspected environment.

## Specialized Skills and Capabilities

Different AI agents may have different skills, tools, plugins, and workflows.

Use available specialized capabilities when their purpose matches the user's request.

Do not assume that a capability available in one agent is available in another.

Do not duplicate specialized workflows unnecessarily.

For example, if a specialized skill named `guideme` is available, it may own:
- Codebase investigation.
- Impact analysis.
- Implementation planning.
- Verification planning.
- Open technical decisions.

If unavailable, use general capabilities to help with the task without pretending that the skill exists.

GUIDE.md defines shared collaboration principles, not the internal implementation of individual agent capabilities.

---

# 5. GUIDE.md – Collaboration and Learning Protocol

This document defines how AI agents and the developer work together.

Keep it technology-agnostic and independent of specific project architecture or AI tools.

## 5.1 Working Modes

Support three flexible working modes.

### Mode A: Independent Implementation & Comparison

This is the preferred mode when the developer wants to learn.

Workflow:

1. Developer describes the desired functionality or problem.
2. AI helps clarify requirements and explain relevant concepts.
3. Developer independently analyzes and implements the solution in the actual application.
4. AI may optionally create a reference implementation in `.scratch/drafts/`.
5. The reference implementation must not be presented as the required solution.
6. The developer completes their implementation before using the reference for comparison.
7. AI compares both implementations.
8. AI identifies and classifies relevant differences.
9. Developer evaluates the differences and makes final decisions.
10. Relevant knowledge is captured in notes.

The reference implementation is optional.

It must never be placed in production code or automatically copied into the developer's implementation.

The reference should preferably be created or inspected only after the developer has completed their own implementation.

Classify differences as:
- Accidental mistake.
- Deliberate design decision.
- Alternative implementation.
- Potential improvement.
- Unresolved question.

The objective is understanding and engineering judgment, not identical implementations.

### Mode B: Guided Implementation

When the developer wants to understand and independently implement a change in an existing codebase:

- Use an appropriate available specialized skill or analysis capability.
- Investigate the actual codebase.
- Explain the relevant implementation context.
- Produce an actionable implementation guide when requested.

Do not recreate a specialized skill's detailed workflow unnecessarily.

### Mode C: AI-Assisted Implementation

When the developer explicitly wants an AI agent to implement something:

- Investigate the relevant existing code.
- Respect AI_RULES.md and RULES.md.
- Keep changes within the requested scope.
- Verify relevant behavior.
- Explain significant decisions and deviations.
- Capture meaningful knowledge where appropriate.

The developer reviews and accepts the result.

The developer may switch between modes at any time.

## 5.2 Learning as a Side Effect

Capture meaningful knowledge during development rather than postponing documentation.

Examples:
- Language concepts.
- Framework behavior.
- Architectural concepts.
- Programming techniques.
- Design patterns.
- Debugging insights.
- Non-obvious implementation details.

Use notes/README.md and notes/TEMPLATE.md.

## 5.3 Architecture Learning

When useful, compare alternative architectures or approaches while keeping functional requirements constant.

Explain:
- Structural differences.
- Trade-offs.
- Complexity.
- Maintainability.
- Failure modes.
- When an approach is inappropriate.

Do not introduce unnecessary abstractions into production code merely for learning purposes.

## 5.4 Pattern Learning

Pattern practice is independent of production development.

Use patterns/README.md and patterns/TEMPLATE.md.

## 5.5 Communication

- Be concise by default.
- Respect the developer's experience.
- Explain non-obvious technical decisions.
- Distinguish facts, assumptions, and recommendations.
- Avoid unnecessary process overhead.
- Ask questions when important decisions cannot reasonably be inferred.

---

# 6. AI_RULES.md – Rules for AI Behavior

This document defines expectations for all AI agents working in the repository.

Its primary purpose is to improve AI reliability through observed experience.

Distinguish between:

- Preventive instructions: How AI should work to avoid mistakes.
- Detection mechanisms: How mistakes can be identified.
- Corrective actions: What AI should do after identifying a mistake.

## Rule format

Each rule should contain:

- Unique ID.
- Name.
- Observed problem or failure mode.
- Required AI behavior.
- Prevention strategy.
- Detection method, if available.
- Status.

## Learning from AI mistakes

Whenever an AI mistake is identified:

1. Describe the actual mistake.
2. Determine its underlying cause.
3. Identify whether it was caused by missing context, unclear instructions, incorrect assumptions, ignored constraints, or another factor.
4. Propose a preventive instruction or rule.
5. Determine whether an automated detection mechanism is possible.
6. Ask the developer to approve the proposed change.
7. Add the approved rule to AI_RULES.md.
8. Implement additional checks only when authorized and technically appropriate.

Examples of possible AI failure modes:

- Implementing functionality that already exists.
- Modifying files outside the requested scope.
- Ignoring established architecture.
- Inventing APIs or methods.
- Making assumptions without verification.
- Forgetting relevant tests.
- Producing documentation that does not match actual code.

These examples illustrate possible categories. Do not automatically turn all examples into mandatory rules.

## Important constraints

- All AI agents share the same AI_RULES.md.
- AI agents may identify and propose improvements.
- No AI agent may autonomously modify AI_RULES.md.
- The developer approves all new or modified AI rules.
- Do not add generic rules without a concrete reason.
- Do not assume an instruction guarantees compliance.
- Use automated detection where possible.

Start with an empty rule registry and a short explanation of the process.

---

# 7. RULES.md – Project Quality Rules

This document contains technical and architectural requirements for the actual application.

These rules apply regardless of whether code was written by the developer or an AI agent.

Distinguish between:

- Enforceable rules.
- Guidelines.
- Preferences.

An enforceable rule must contain:

- Unique ID.
- Name.
- What must be true.
- Why it matters.
- Detection method.
- Test or check location.

A rule is not considered automatically enforced until an actual check exists and has been successfully executed.

Examples:
- Architectural dependency constraints.
- API compatibility requirements.
- Error-handling requirements.
- Security constraints.
- Testing requirements.

Do not invent technology-specific rules before understanding the actual project.

Start with an empty rule registry.

---

# 8. TREE.md – Repository Map

Document the actual repository structure.

Clearly distinguish between:

- Application source code.
- Tests.
- Configuration and infrastructure.
- Documentation.
- AI framework files.
- Temporary AI artifacts.
- Learning resources.

Explain:
- Important directories.
- Their responsibilities.
- Where different types of files belong.
- Relevant organizational conventions.

Keep it synchronized with the filesystem.

Exclude irrelevant generated directories and dependencies.

Do not duplicate the project's README.

---

# 9. PROGRESS.md – Project State

Create a lightweight project progress document.

Include:
- Current phase.
- Completed work.
- Current task.
- Next steps.
- Important decisions.
- Open questions.

Only document verified progress.

Do not mark planned work as completed.

Avoid duplicating existing task management systems.

Keep project progress separate from AI framework maintenance.

---

# 10. README.md – Framework Quick Start

Create a short, practical usage guide for the developer.

It must explain:

- The three working modes.
- When to use each mode.
- How independent implementation and comparison work.
- How to handle AI mistakes.
- Where to store notes, patterns, rules, and decisions.
- The purpose of the main framework files.

Keep it simple and concise.

It must be usable regardless of which AI agent the developer chooses.

Avoid duplicating the detailed instructions from GUIDE.md.

If README.md already exists, preserve its original purpose and integrate the instructions appropriately. Do not replace an existing project README with framework documentation.

---

# 11. Notes System

Create notes/README.md and notes/TEMPLATE.md.

Notes capture reusable knowledge acquired during development.

Organize them by meaningful domains, languages, techniques, concepts, or other relevant categories.

Each note must contain:

## What
What is the concept?

## Why
What problem does it solve?

## Example
A practical example, preferably from the actual application.

## Gotchas
Limitations, mistakes, edge cases, and important considerations.

## Used in
Actual repository paths where the concept is used.

Explain:
- When notes should be created.
- When existing notes should be updated.
- How to organize notes.
- How to reference actual implementation files.
- Why knowledge should be captured during development.
- Why notes should remain practical and concise.

Do not create artificial learning notes.

---

# 12. Pattern Learning System

Create patterns/README.md and patterns/TEMPLATE.md.

Patterns are studied independently from production code.

For each pattern, support:

1. Canonical implementation.
2. Realistic variations.
3. Implementations in different programming languages where useful.
4. Failure cases and limitations.
5. Trade-off analysis.
6. When the pattern should and should not be used.

The pattern template must include:

- Problem solved.
- Canonical structure.
- Variations.
- Language comparison.
- Trade-offs.
- Failure cases.
- When not to use.
- Used in.

The README must explain how to learn patterns through active implementation rather than passive reading.

Do not create example patterns yet.

Pattern practice must not automatically introduce these patterns into the actual application.

---

# 13. Architecture Decision Records

Create docs/decisions/TEMPLATE.md.

Use a lightweight format:

- Context.
- Decision.
- Alternatives considered.
- Consequences.

Explain when an ADR is appropriate.

Do not create ADRs for trivial implementation details.

Do not invent architectural decisions during framework initialization.

---

# 14. Important Design Principles

The framework must:

- Be technology-agnostic.
- Be project-independent.
- Be AI-agent-independent.
- Be reusable across repositories.
- Adapt to existing application structure.
- Keep application code separate from temporary AI artifacts.
- Keep developer learning separate from AI behavior improvement.
- Keep AI behavior rules separate from project quality rules.
- Respect agent-specific capabilities without depending on them.
- Avoid unnecessary process overhead.
- Grow through actual experience.
- Prefer practical improvements over theoretical rule collections.

Do not:
- Implement application functionality.
- Install dependencies.
- Reorganize existing application code.
- Force a generic application directory structure.
- Invent project-specific architecture.
- Invent project-specific quality rules.
- Create artificial learning notes.
- Create artificial architectural decisions.
- Allow AI agents to autonomously change shared AI rules.
- Require a reference implementation for every task.
- Assume a specific AI agent or provider.

---

# 15. Final Validation

After initialization:

1. Inspect all created and modified files.
2. Verify that documents do not contradict one another.
3. Verify that AI.md provides a clear shared entry point.
4. Verify that the framework does not depend on a specific AI provider or tool.
5. Verify that agent-specific instruction files, if created, are thin adapters.
6. Verify that existing application code and architecture remain unchanged.
7. Verify that independent implementation happens before reference comparison.
8. Verify that AI behavior rules and project quality rules are clearly separated.
9. Verify that AI rules require developer approval before modification.
10. Verify that templates are technology-agnostic.
11. Verify that `.scratch/` is ignored by Git.
12. Verify that TREE.md reflects the actual filesystem.
13. Verify that README.md explains daily usage concisely.
14. Report all created and modified files.
15. Report assumptions and unresolved questions.

Do not begin implementing application functionality.

The task is complete when the repository contains a coherent, reusable, AI-agent-independent development and learning framework that integrates with the existing application without restructuring it.
