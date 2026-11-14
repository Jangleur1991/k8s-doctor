---
name: guideme
description: Analyzes a bug, feature request, or change ($ARGS) against the current codebase and produces a step-by-step implementation guide — an "onboarding ticket" — that a senior engineer follows to write the code themselves. Use when the user wants to understand and own a change rather than delegate it, asking to be "guided", "walked through", or "explain where/what to change" instead of having the agent write the code.
---

# Guide

You produce an **implementation guide**, not an implementation. The human writes
the code; you draw the map and explain the terrain. Think "well-written onboarding
ticket handed to a senior engineer who is new to *this* codebase" — not a tutorial.

The request to handle is: $ARGS

## Hard rules

- **MUST NOT** create, edit, or write production code, config, or tests. You investigate and explain only.
- **MUST** read the actual codebase before writing the guide. Never guess at file names, function signatures, or call sites — locate them.
- The reader is a senior engineer. Explain *architecture, intent, trade-offs, and non-obvious coupling*. **DO NOT** explain language syntax, standard library basics, or general programming concepts. The goal **IS NOT** guiding Junior developer, the gaol **IS** onboard the Senior engineer to the codebase.
- Point to **specific anchors**: file path + function/class/block (and line range if stable). "Somewhere in the auth layer" is a failure.
- When something is genuinely ambiguous, surface it as a decision for the human — do not silently pick.

## Workflow

Copy this checklist into your response and check items off as you go:

Guide Progress:

- [ ] 1. Restate the request and confirm scope
- [ ] 2. Map the relevant slice of the codebase
- [ ] 3. Trace the change end-to-end (entry point → impact)
- [ ] 4. Draft the step-by-step guide
- [ ] 5. Define verification
- [ ] 6. List open decisions

**1. Restate & scope.** In 1–2 sentences, state what $ARGS actually asks for and what is in/out of scope. If $ARGS is too vague to scope, ask up to 3 sharp questions and stop.

**2. Map the slice.** Locate only the parts of the system this change touches: entry points, the modules/functions involved, data flow, and existing patterns/conventions the change should mirror. Use search/grep over the repo; read the real files. Build just enough mental model to explain the *why*, not a tour of the whole repo.

**3. Trace end-to-end.** Follow the change from its entry point to every place it ripples: callers, shared state, persistence, public contracts/APIs, tests that will break. Note hidden coupling the reader wouldn't expect.

**4. Draft the guide** using the output template below.

**5. Verification.** Specify how the human confirms the change works: which tests to run or add, what to check manually, what "done" looks like.

**6. Open decisions.** List the judgment calls you deliberately left to the human, each with the options and your recommendation.

## Output template

Produce the guide in exactly this structure:

```markdown
# Guide: short title of the change

## TL;DR
One sentence: what changes and why. One sentence: the core approach.

## Mental model
The minimum the reader needs about how this part of the system works today:
the relevant flow, the convention to follow, the constraint that matters.
Explain intent and trade-offs, not basics.

## Change plan
For each step:

### Step N — imperative summary

- **Where:** `path/to/file.ext` → `functionOrBlock` (lines ~A–B)
- **What:** Change behavior from **X** to **Y**.
- **Why:** the reasoning — what this enables, what it prevents, the trade-off chosen;
- **Watch out:** non-obvious coupling, edge case, or thing that will silently break;

(Order steps so the code stays in a buildable/coherent state where possible.
Call out the one step that is the "load-bearing" change vs. the mechanical follow-ons.)

## Verification

- tests to run / add, manual checks, expected observable outcome;

## Open decisions

- **decision:** options — *Recommended: X, because reason.
