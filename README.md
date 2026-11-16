# k8s-doctor


## TODO

- Try out the guid me skill
- Read the brainstorm plan
- Try to establish something like this in order to establish a thoughtful ai driven development: https://www.reddit.com/r/Backend/comments/1vvejv3/how_do_you_keep_learning_as_an_sde_when_ai_does/

## AI development framework

Working with AI agents here follows a shared framework. Start with `AI.md` (agent entry point), `GUIDE.md` (working modes) and `AI_RULES.md` (AI behaviour rules). Project requirements are in `RULES.md`, the repo map in `TREE.md`, and current status in `PROGRESS.md`.

Pick a working mode per task:

- **Independent** — you implement, then compare with an optional AI reference in `.scratch/drafts/`.
- **Guided** — the AI maps the code and writes a guide; you implement.
- **AI-assisted** — the AI implements; you review and accept.

Store learnings in `notes/`, pattern practice in `patterns/`, and architecture decisions in `docs/decisions/`. Changes to AI rules need your approval. `.scratch/` is git-ignored.
