# Setup and Usage Checklist

Use this checklist when creating the OpenBot development team and when running work through it.

---

## Phase 1 — Initial Setup

### OpenBot Installation
- [ ] OpenBot installed and opened
- [ ] Provider connected (Claude, Codex, Grok, etc.)

### Create Agents
- [ ] Spec Writer created and instructions from `roles/spec-writer.md` pasted
- [ ] Architect created (optional at start)
- [ ] Implementer created and instructions from `roles/implementer.md` pasted
- [ ] Reviewer created and instructions from `roles/reviewer.md` pasted
- [ ] Docs created (optional at start)
- [ ] Project Control created and instructions from `roles/project-control.md` pasted
- [ ] Observability created and instructions from `roles/observability.md` pasted

### Model and Reasoning Effort
- [ ] Spec Writer: strong model, medium effort
- [ ] Implementer: strong coding model, medium effort
- [ ] Reviewer: strong model, medium effort
- [ ] Project Control: fast model, low effort
- [ ] Observability: fast model, low effort
- [ ] Backup models configured for agents that may hit rate limits

### Skills (optional but recommended)
- [ ] poteto-mode / pstack available
- [ ] Matt Pocock skills available (grilling, to-spec, to-tickets, tdd, code-review, retro)

### Channels
- [ ] Decide whether to use one set of channels or channels per project
- [ ] `#spec-to-impl` (or `#projeto-x-spec-to-impl`) created and agents added
- [ ] `#impl-review` created and agents added
- [ ] `#project-status` created
- [ ] `#observability` created
- [ ] Optional: `#docs` and `#lessons`

**Note**: Agents are created once and reused. Channels are cheap to create and can be named per project.

### GitHub
- [ ] GitHub plugin/MCP connected on Spec Writer, Implementer, Reviewer, Project Control
- [ ] Labels created: `spec`, `ticket`, `bug`, `docs`, `needs-evidence`, `blocked`
- [ ] Projects board created with columns: Spec / Ready for Impl / In Progress / In Review / Done
- [ ] Branch protection on main (recommended)

### Routines
- [ ] Project Control: daily status routine enabled
- [ ] Observability: evidence and traceability check routine enabled

---

## Phase 2 — Running a Ticket (per feature or bug)

### Alignment
- [ ] Talked to Spec Writer with clear intent and observable success criteria
- [ ] Grilling completed if needed
- [ ] No open questions left

### Spec and Tickets
- [ ] Parent Spec issue created (label `spec`)
- [ ] Child Ticket issues created (label `ticket`) and linked to parent
- [ ] Blocking edges clear
- [ ] Handoff posted in `#spec-to-impl`

### Architecture (if needed)
- [ ] Architect reviewed seams
- [ ] ADR created if decision is significant

### Implementation
- [ ] Implementer worked only on the ticket
- [ ] Real evidence produced (command + output or equivalent)
- [ ] Draft PR opened with `Closes #<ticket>` and evidence in the body
- [ ] Branch named `ai/<ticket-number>-<slug>`

### Review
- [ ] Reviewer checked standards, spec fidelity, and evidence
- [ ] Reviewer verified the ticket links to a parent spec
- [ ] Clear decision: Approve or Request changes

### Observability
- [ ] Evidence present
- [ ] CI green (or issues flagged)
- [ ] Traceability to spec confirmed

### Documentation
- [ ] Docs updated if relevant (changelog, examples, GLOSSARY)

### Lessons Learned (when there was a correction)
- [ ] Reflection requested (Accepted / Rejected / Backlog)
- [ ] Durable lessons recorded in `LESSONS.md` or encoded structurally

### Project Control
- [ ] Board updated
- [ ] Daily status reflects the new state

---

## Phase 3 — Ongoing Health

- [ ] Daily status from Project Control is useful
- [ ] Evidence checks from Observability catch missing proof
- [ ] Specs stay separate from tickets
- [ ] You can see progress from approved requirements to done work
- [ ] Agent stops are handled with resume or small tickets
- [ ] Backup models prevent silent stops on rate limits

---

## Quick Decision Tree

- Idea is vague → Spec Writer
- Need tickets → Spec Writer (parent Spec + child Tickets)
- Ticket is clear → Implementer
- PR is open → Reviewer
- Want status → Project Control
- Want evidence/CI check → Observability
- Something was corrected → Reflection + LESSONS.md

Print or keep this checklist open while setting up and while running the first few tickets.
