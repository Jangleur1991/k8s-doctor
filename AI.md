# AI Entry Point

This is the shared, tool-independent entry point for every AI agent working in this repository. Read it first.

## Documents and when to read them

| Document | Read when |
|---|---|
| `GUIDE.md` | Always, once per session. Collaboration and learning principles, working modes. |
| `AI_RULES.md` | Always, once per session. Rules for how the AI behaves here. |
| `RULES.md` | Before any relevant code or structural change. Project quality requirements. |
| `TREE.md` | Before navigating or changing the repository layout. |
| `PROGRESS.md` | When current project status matters (planning, "what's next", resuming work). |
| `notes/` | When a concept or technique relevant to the task may already be documented. |
| `docs/decisions/` | When a change touches an existing architectural decision. |
| `patterns/` | Only when the developer asks to practise a pattern. Pattern practice is separate from production code. |

## Three concerns, kept separate

Every action serves one of these. Do not mix them up:

1. **Improving AI behaviour** — changes to `AI_RULES.md`. Only with developer approval.
2. **Helping the developer learn** — explanations, comparisons, notes. The developer writes production code unless they explicitly ask otherwise.
3. **Maintaining project quality** — requirements in `RULES.md`, checked by real tests or checks.

## Working rules (summary)

- Follow the working mode the developer chose (see `GUIDE.md` §5.1). If none is named, ask one short question.
- Stay inside the requested scope. Do not reorganise existing code.
- Investigate the real code before answering questions about it. Do not guess file names, APIs or behaviour.
- Put reference implementations and review artifacts only in `.scratch/drafts/` and `.scratch/reviews/`. Never in application code.
- Never modify `AI_RULES.md` on your own. Propose changes; the developer approves.
- Distinguish facts, assumptions and recommendations in your output.
- If a specialised skill matches the task (for example `guideme` for an implementation guide), use it when it is available. Do not claim a skill exists when it does not.

## Agent independence

This file is the source of truth for repository-level AI instructions. Agent-specific files (`CLAUDE.md`, `AGENTS.md`) are thin adapters that point here. Do not copy rules into them.

Do not assume a specific provider, model, CLI, IDE or agent. Capabilities differ between agents; use what is present and do not depend on it.
