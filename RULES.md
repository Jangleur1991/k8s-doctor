# Project Quality Rules

Technical and architectural requirements for the application. They apply whether the developer or an AI agent wrote the code.

Kept separate from `AI_RULES.md` (how AI behaves). The developer owns which requirements are mandatory.

## Categories

- **Enforceable rule:** must be true; has a detection method and a check location. Not "enforced" until a real check exists and has run successfully.
- **Guideline:** recommended practice, not checked automatically.
- **Preference:** personal or stylistic choice.

## Entry format

```
### RUL-NNN — Name  [enforceable | guideline | preference]
- What must be true:
- Why it matters:
- Detection method:
- Check location: (file/command, or "none yet")
- Status: proposed | active | retired
```

## Rule registry

*Empty. Rules are added only after they are grounded in the actual project. Do not invent technology-specific rules ahead of the code.*
