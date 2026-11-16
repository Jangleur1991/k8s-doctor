# AI Behaviour Rules

Rules for how AI agents work in this repository. They exist to reduce recurring AI mistakes.

All agents share this file. **No AI agent may modify this file autonomously.** The developer approves every new or changed rule.

## Process

Preventive instructions, detection mechanisms and corrective actions are distinct:
- **Preventive:** how the AI should work to avoid a mistake.
- **Detection:** how the mistake can be identified (ideally automated).
- **Corrective:** what the AI does once it has made one.

### Learning from AI mistakes

When a mistake is identified:

1. Describe the actual mistake.
2. Determine its cause: missing context, unclear instructions, incorrect assumption, ignored constraint, or other.
3. Propose a preventive rule.
4. Decide whether automated detection is possible.
5. Ask the developer to approve the proposal.
6. Only after approval, add the rule below.
7. Add automated checks only when authorised and technically appropriate.

Known failure categories (illustrative, not automatically mandatory rules): implementing existing functionality, editing files outside scope, ignoring architecture, inventing APIs, unverified assumptions, forgotten tests, documentation that does not match code.

Do not add generic rules without a concrete observed reason.

## Rule format

```
### AIR-NNN — Name
- Observed problem:
- Required behaviour:
- Prevention:
- Detection: (method, or "none yet")
- Status: proposed | approved | retired
- Approved by / date:
```

## Rule registry

*Empty. No rules have been observed or approved yet.*
