# Analysis: Lessons Learned and Error Correction (poteto + Matt Pocock)

## Why this matters

AI models repeat errors. Without an explicit mechanism to capture and encode lessons, the team relearns the same things every session.  
Both poteto and Matt Pocock treat this as a central part of the process — not as an afterthought.

## How poteto handles it

| Mechanism | What it does | When to use |
|-----------|--------------|-------------|
| `/reflect` | Mines the conversation transcript with parallel subagents. Extracts durable learnings and proposes concrete edits to existing skills. Presents Accepted/Rejected/Backlog and waits for human approval. | After a complex task, user correction, or new workflow worth capturing. |
| Principle `encode-lessons-in-structure` | Prefers lint, script, metadata flag, or runtime check over more prose in a skill. | Whenever a lesson can be enforced structurally. |
| `/correct` (mentioned in guides) | When the same error is corrected repeatedly, the fix goes into the repo/environment, not the next prompt. | Recurring errors. |
| Origin of principles | Many pstack principles were born from months of real agent error corrections. | — |

**Strength**: the skill improvement loop is explicit and multi-agent. Preference for structural enforcement reduces dependence on context memory.

**Limitation**: depends on the user (or agent) invoking `/reflect`. Not automatic at the end of every task.

## How Matt Pocock handles it

| Mechanism | What it does | When to use |
|-----------|--------------|-------------|
| `/retro` | Reads the session record, finds where the agent struggled, and suggests improvements to the **environment** (navigation pointers, automated checks, coding standards, AGENTS.md cleanup). Ordered by severity. Uses `writing-for-agents`. | At the end of an implementation or review session. |
| Focus on environment | Does not change the feature code. Changes what the agent sees and what the system forces next time. | Whenever the error is recurring or expensive. |

**Strength**: clearly separates “improve the code” from “improve the agent’s environment”. Prioritizes automated checks over prose rules.

**Limitation**: also user-invoked. Does not run by itself.

## Synthesis for the OpenBot team

Both address the same problem in complementary ways:

- **poteto `/reflect`** → improves the **skills** (how the agent works).
- **Matt `/retro`** → improves the **environment** (checks, navigation, standards).

In the OpenBot team this should become an explicit routine, not something that depends on someone remembering to call the command.

### Recommended lessons-learned flow

1. **During the phase**  
   When a human or another agent corrects something, the phase agent notes it in memory (or in the channel).

2. **At the end of the relevant phase** (especially after Implementation + Review)  
   - Implementer or Reviewer can be instructed to propose reflections.  
   - Or a dedicated agent (or Project Control) triggers a structured “reflect / retro”.

3. **Destinations for lessons** (preference order)
   1. Structural enforcement (lint, test, CI, script) — `encode-lessons-in-structure`
   2. Agent skill (via edit proposed by `/reflect`)
   3. Agent memory + GLOSSARY/ADR if it is domain knowledge
   4. AGENTS.md / standards only if it is judgment that cannot be automated (Matt’s preference)

4. **Human approval**  
   Always present Accepted/Rejected/Backlog (poteto style) or a prioritized list (Matt style) before applying.

### How this appears in the roles

- **Implementer**: at the end of a ticket with corrections, can be asked to generate a reflection.
- **Reviewer**: when finding an error pattern, signals for reflection.
- **Project Control**: can have a weekly “lessons of the week” routine.
- **All**: memories should record recurring corrections.
- **Docs**: can maintain a `LESSONS.md` or section in ADRs with process decisions.

### Intentional prompt for lessons learned

```
@Implementer (or @Reviewer) Reflect on the session for ticket #124.
What errors were corrected?
Which ones can be avoided structurally (lint, test, skill)?
Present in Accepted / Rejected / Backlog format and wait for approval before applying any change.
```

Or, if the agent has the skills installed:

```
@Implementer /reflect on what we learned in this ticket.
```

```
@Implementer /retro — suggest environment improvements so we don't repeat this.
```

---

**Conclusion**: poteto and Matt treat lessons learned explicitly and oriented toward changing the environment/skills. In the OpenBot team this should be a conscious phase of the cycle, not something that happens by chance. The detailed guides incorporate this.
