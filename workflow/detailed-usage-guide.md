# Detailed Usage Guide: How, When, and Why to Talk to Each Agent

This is the main operational guide.  
The agents use Matt Pocock’s and poteto’s skills under the hood.  
You talk to the **agent that owns the phase**, declaring the **desired result**.

## Communication Principles

1. **One agent per phase** — do not mix responsibilities in the same prompt.
2. **Observable result** — say what counts as done.
3. **Minimum sufficient context** — link to issue/spec/ADR instead of pasting everything.
4. **Structured handoff** — the agent should post in the channel what was done and the next step.
5. **Lessons learned** — after relevant corrections, ask for reflection (see final section).

## Channels vs Private Chats

**Rule of thumb**:
- **Channel** = coordination and handoffs (visibility for the team)
- **Private chat** = heavy work and long context

| Situation | Use | Why |
|-----------|-----|-----|
| Alignment of intent | Channel `#spec-to-impl` | Spec Writer, Architect and Project Control see the same history |
| Spec → Implementation handoff | Channel | Implementer already sees what was decided |
| Heavy implementation (long code) | Implementer's private chat | Avoids polluting the channel and blowing other agents' context |
| PR review | Channel `#impl-review` | Reviewer and Observability see the evidence and decision |
| Daily status / blockers | Channel `#project-status` | Visibility for everyone |
| Reflection / lessons | Channel `#lessons` or private chat | Depends on whether you want the team to see it |
| Point question to one agent | Private chat | No need for the whole team to see |
| Multiple projects | Channels per project | Isolates history without duplicating agents |

**Recommended pattern**:
1. Start in the channel with a clear prompt and mention (`@SpecWriter ...`).
2. Let the agent work and post the handoff in the channel.
3. If the reply is very long (code, large analysis), ask it to continue in the private chat and only summarize in the channel.
4. Mention the next agent in the handoff (`@Implementer can pick up #124`).

**When a channel gets long**:
- Ask Project Control for a summary, or
- Archive the channel and open a new one for the next phase.

**Avoid**:
- Letting every agent reply to every message.
- Pasting huge code or logs in the channel.
- Using the channel as the only place for long tasks.
- Creating a new agent just because you switched projects.

---

## Complete Phase Map

### Phase 0 — Before anything (optional)

**Who**: Project Control  
**When**: You want to understand the current state of the project.  
**Why**: Avoids working on something already in progress or blocked.

**Prompt**:
```
@ProjectControl What is the current board status?
List In Progress, In Review, and blocked items with links.
```

---

### Phase 1 — Alignment / Idea

**Main agent**: Spec Writer  
**When**: Intent is still vague, ambiguous, or you want a stress-test before committing.  
**Why**: Uses grilling (Matt) + investigation (poteto) to force decisions and build domain understanding.

**Detailed prompt**:
```
@SpecWriter I want [objective in one sentence].

Users: [who].
Current problem: [concrete pain].
Observable success criteria:
- [measurable criterion 1]
- [measurable criterion 2]

Do grilling if needed.
Do not write code.
Desired result: clear decisions + GLOSSARY update if there are new terms.
```

**What it does under the hood**: grilling / grill-with-docs, domain-modeling, poteto investigation playbook.

**Expected handoff**:
```
## Alignment complete
Main decisions: ...
GLOSSARY updated: yes/no
Next: can I generate spec + tickets?
```

---

### Phase 2 — Specification and Tickets

**Main agent**: Spec Writer  
**When**: Alignment has already happened.  
**Why**: to-spec + to-tickets (Matt) generate trackable work on GitHub.

**Prompt**:
```
@SpecWriter Turn the previous conversation/alignment into a parent Spec issue and child Ticket issues.
Publish to GitHub.
Done = one Spec issue (label spec) and linked Ticket issues (label ticket) with observable acceptance criteria and clear blocking edges.
Update GLOSSARY/ADRs if needed.
```

**Expected handoff** (it posts in the channel):
```
## Handoff Spec → Impl
Parent Spec: #123 — Spec: [short name]
Tickets:
- #124 — Ticket: [name] (no blockers)
- #125 — Ticket: [name] (blocked by #124)
GLOSSARY: updated
ADRs: [links if any]
@Architect review seams if needed
@ProjectControl update the board (keep spec and tickets separate)
```

---

### Phase 3 — Architecture / Domain

**Main agent**: Architect  
**When**: You need a design decision, test seams, or ADR.  
**Why**: domain-modeling + codebase-design + poteto boundary principles.

**Prompt**:
```
@Architect Review the design for feature #123 / tickets #124-#125.
Define test seams.
Create ADR if the decision is significant.
Desired result: clear seams, GLOSSARY updated, and impact on tickets documented.
```

---

### Phase 4 — Implementation

**Main agent**: Implementer  
**When**: Ticket is clear, no blockers, and you want code + evidence.  
**Why**: TDD (Matt) + poteto-mode / prove-it-works. You don't need to remember the names.

**Detailed prompt**:
```
@Implementer Implement ticket #124.
Originating spec: #123
Agreed seams: [list, if any]
Done = real evidence of the artifact (command run + output / screenshot / log).
Do not implement beyond the ticket.
Open a draft PR with:
- Closes #124
- Acceptance criteria marked
- Evidence in the body
- Principles used
```

**What it does under the hood**: tdd, poteto-mode (prove-it-works, sequence-verifiable-units, feature or bug-fix playbook).

**Expected handoff**:
```
## PR Ready for Review
PR: #456 (draft)
Issue: #124
Evidence: [summary + links]
Tests: red-green on seams
CI: green
@Reviewer can review
@Observability confirm evidence
```

---

### Phase 5 — Review

**Main agent**: Reviewer  
**When**: PR is open with evidence.  
**Why**: Independent review. Two-axis code-review (Matt) + real verification (poteto).

**Prompt**:
```
@Reviewer Review PR #456.
Check:
- Standards (style, smells, conventions)
- Fidelity to spec #123 / ticket #124
- Real evidence of the artifact
- That the PR closes a ticket linked to a parent spec
Clear decision: Approve or Request changes.
If you find an error pattern, signal for reflection.
```

---

### Phase 6 — Documentation

**Main agent**: Docs  
**When**: After domain decisions, ADRs, or relevant merges.  
**Why**: Keeps GLOSSARY, ADRs, changelog, and examples alive (unslopped prose).

**Prompt**:
```
@Docs Update changelog, examples, and relevant documentation from feature #123 / PR #456 (merged).
Source: [links].
Keep coherence with GLOSSARY.
```

---

### Phase 7 — Project Control

**Main agent**: Project Control  
**When**: You want status, blockers, board update, or cleanup.  
**Why**: Owner of visibility on GitHub.

**Prompt**:
```
@ProjectControl What is the current status?
List In Progress, In Review, and blocked with links.
Move what is already done.
```

---

### Phase 8 — Observability

**Main agent**: Observability  
**When**: You want to know if CI is green or if evidence is missing.  
**Why**: Applies prove-it-works continuously.

**Prompt**:
```
@Observability Check open PRs.
Alert those without real evidence or with failing CI.
```

---

## Cross-cutting Phase — Lessons Learned

**When**: After a relevant correction, end of a complex ticket, or observed error pattern.  
**Why**: poteto (`/reflect` + encode-lessons-in-structure) and Matt (`/retro`) exist exactly for this. Without it the team repeats the same errors.

**Who can trigger**:
- Implementer (after corrections on the ticket)
- Reviewer (when seeing a pattern)
- Project Control (weekly routine)
- You yourself, at any time

**Intentional prompt**:
```
@Implementer Reflect on the session for ticket #124.
What errors were corrected?
Which ones can be avoided structurally (lint, test, CI, skill)?
Present in Accepted / Rejected / Backlog format.
Wait for my approval before applying any change to skills or environment.
```

**Or, if the agent has the skills installed**:
```
@Implementer /reflect on what we learned in this ticket.
```
```
@Implementer /retro — suggest environment improvements so we don't repeat this.
```

**Preferred destinations for lessons** (order):
1. Structural enforcement (lint, test, CI)
2. Skill edit (via reflect)
3. Agent memory + GLOSSARY/ADR
4. AGENTS.md / standards only if pure judgment

---

## Channels and When to Use Them

| Channel            | Main use                               | Main participants                         |
|--------------------|----------------------------------------|-------------------------------------------|
| `#spec-to-impl`    | Alignment, specs, tickets, handoff     | Spec Writer, Architect, Implementer, Project Control |
| `#impl-review`     | Implementation and review              | Implementer, Reviewer, Observability      |
| `#project-status`  | Status and blockers                    | Project Control + everyone                |
| `#docs`            | Documentation updates                  | Docs, Spec Writer, Architect              |
| `#observability`   | CI and evidence                        | Observability, Reviewer                   |
| `#lessons`         | Reflections and lessons                | All (optional)                            |

---

## Quick Reference

See `workflow/detailed-quick-reference.md` for a compact table of who to talk to and when.
