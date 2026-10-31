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
@SpecWriter Turn the previous conversation/alignment into a spec and tickets.
Publish to GitHub.
Done = issues created with observable acceptance criteria and clear blocking edges.
Update GLOSSARY/ADRs if needed.
```

**Expected handoff** (it posts in the channel):
```
## Handoff Spec → Impl
Main issue: #123
Tickets:
- #124: [description] (no blockers)
- #125: [description] (blocked by #124)
GLOSSARY: updated
ADRs: [links if any]
@Architect review seams if needed
@ProjectControl update the board
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
| #spec-to-impl      | Alignment, specs, impl handoffs        | Spec Writer, Architect, Implementer, Project Control |
| #impl-review       | PRs, reviews, evidence                 | Implementer, Reviewer, Observability      |
| #project-status    | Status, blockers                       | Project Control + general reading         |
| #docs              | Documentation updates                  | Docs + Spec Writer / Architect            |
| #observability     | CI and evidence alerts                 | Observability + Reviewer                  |
| #lessons           | Reflections and lessons learned (optional) | All                                     |

Prefer the phase channel. The history stays shared and trackable.

---

## Anti-patterns

- Loose prompt (“make the login better”).
- Talking to the Implementer before a clear ticket.
- Asking the Reviewer to implement.
- Not declaring what counts as done.
- Ignoring lessons learned after corrections.
- Mixing phases in the same prompt.

---

## One-sentence summary per agent

- **Spec Writer** → Turn intent into a trackable spec and tickets.
- **Architect** → Decide domain, seams, and ADRs.
- **Implementer** → Implement the ticket with TDD and prove it works.
- **Reviewer** → Review independently and confirm the evidence.
- **Docs** → Keep documentation alive and coherent.
- **Project Control** → Tell me where everything is and what is blocked.
- **Observability** → Verify that it is actually working and alert what is missing.

With this guide you talk with clear intent. The agents take care of using Matt’s and poteto’s skills — including the lessons-learned ones — at the right moment.
