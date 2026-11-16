# Collaboration and Learning Protocol

This document defines how AI agents and the developer work together in this repository. It is technology-agnostic and does not describe any specific application architecture or tool.

Three goals, held independently: developer learning, AI collaboration quality, and project quality. See `AI.md` for how they are kept apart.

## 5.1 Working modes

The developer picks a mode per task and may switch at any time.

### Mode A — Independent implementation and comparison

Preferred when the developer wants to learn.

1. Developer describes the functionality or problem.
2. AI clarifies requirements and explains relevant concepts. It does not write the solution.
3. Developer analyses and implements it in the actual application.
4. *Optional:* AI creates a reference implementation in `.scratch/drafts/`.
5. The reference is never presented as the required solution.
6. Developer finishes their implementation **before** looking at the reference.
7. AI compares both implementations.
8. AI classifies relevant differences (see below).
9. Developer evaluates the differences and decides.
10. Relevant knowledge goes into `notes/`.

Classification of differences:
- Accidental mistake
- Deliberate design decision
- Alternative implementation
- Potential improvement
- Unresolved question

The goal is understanding and engineering judgement, not identical code. A reference is never required, and it is never copied into production code.

### Mode B — Guided implementation

Preferred when the developer wants to understand and write a change in an existing codebase.

- Use an available specialised analysis capability if one matches (for example the `guideme` skill). Otherwise investigate directly.
- Investigate the real code, explain the implementation context, and produce an actionable guide when requested.
- The developer writes the code.

### Mode C — AI-assisted implementation

Used when the developer explicitly asks the AI to implement.

- Investigate the relevant existing code first.
- Respect `AI_RULES.md` and `RULES.md`.
- Keep changes within the requested scope.
- Verify the behaviour (tests or observable checks) and report what was verified.
- Explain significant decisions and deviations.
- Capture meaningful knowledge in `notes/` where appropriate.
- The developer reviews and accepts the result.

## 5.2 Learning as a side effect

Capture knowledge while working, not later. Worth capturing: language or framework behaviour, architectural concepts, techniques, design patterns, debugging insights, non-obvious implementation details.

Use `notes/README.md` and `notes/TEMPLATE.md`. Do not create notes just to have notes.

## 5.3 Architecture learning

When useful, compare alternative approaches while keeping functional requirements fixed. Explain structure, trade-offs, complexity, maintainability, failure modes, and when an approach is inappropriate.

Do not add abstractions to production code purely for learning.

## 5.4 Pattern learning

Pattern practice is separate from production work. Use `patterns/README.md` and `patterns/TEMPLATE.md`. Practising a pattern never introduces it into the application.

## 5.5 Architecture decisions

Record significant, durable decisions in `docs/decisions/` using `TEMPLATE.md`. Trivial implementation details do not get an ADR. Do not invent decisions the developer has not made.

## 5.6 Communication

- Concise by default.
- Respect the developer's experience; explain non-obvious decisions, not basics.
- Separate facts, assumptions and recommendations.
- Ask when an important decision cannot reasonably be inferred.
- Avoid process overhead that does not help the task.

## 5.7 When something goes wrong

If the AI makes a mistake, follow the process in `AI_RULES.md` ("Learning from AI mistakes"). Do not silently fix and move on, and do not change the rules without approval.
