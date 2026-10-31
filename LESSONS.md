# LESSONS.md

Template for lessons learned for the OpenBot development team.

> This file is the capture point for durable learnings.  
> Update it whenever a relevant correction happens or at the end of an important phase.  
> Prefer structural enforcement (lint, test, CI, script) over prose alone.

## How to use

1. After a correction or complex session, ask the phase agent for a reflection:
   ```
   @Implementer Reflect on ticket #124.
   Present in Accepted / Rejected / Backlog format.
   ```
2. Review and approve what is durable.
3. Record it here and, if possible, encode it structurally.

## Entry format

```markdown
### [Date] — [Ticket / PR / Context]

**What happened**  
Short description of the error or friction.

**Why it happened**  
Root cause (if known).

**How to avoid next time**  
- [ ] Structural enforcement (lint, test, CI, script)
- [ ] Skill edit
- [ ] Memory / GLOSSARY / ADR update
- [ ] Rule in standards (only if it cannot be automated)

**Status**  
Accepted / Rejected / Backlog
```

## Recorded lessons

<!-- Add below -->

### Example — Ticket #000 — Prompt without explicit done

**What happened**  
Agent implemented beyond scope because done was not clear.

**Why it happened**  
Loose prompt without observable criteria.

**How to avoid next time**  
- [x] Always declare “Done = real evidence of [X]” in implementation prompts.
- [ ] Add checklist to PR template.

**Status**  
Accepted
