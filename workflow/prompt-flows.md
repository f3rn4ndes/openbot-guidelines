# Concrete Prompt Flows and Handoffs (OpenBot Channels)

This document describes the end-to-end flow from specification to implementation to review, with handoffs in shared Channels.

## Recommended Channels

- `#spec-to-impl`: Spec Writer, Architect, Implementer, Project Control
- `#impl-review`: Implementer, Reviewer, Observability, Project Control
- `#project-status`: Project Control + everyone (read)
- `#docs`: Documentation + Spec Writer / Architect
- `#observability`: Observability + Reviewer

## Main Flow

### 1. Intent → Spec (Human or agent starts)

**Prompt in `#spec-to-impl` (to Spec Writer):**
```
@SpecWriter I want to add JSON export support to the report command.
Users: developers who consume the output in pipelines.
Current problem: text only, hard to parse.
Observable success criteria:
- Text remains identical (byte-identical when possible)
- Valid and parseable JSON
- Both running against the sample project
Do grilling if needed and produce spec + tickets.
```

**Spec Writer responds with grilling if needed, then handoff:**
```
## Handoff Spec → Impl
Parent Spec: #123 — Spec: JSON export
Tickets:
- #124 — Ticket: Add --json flag (no blockers)
- #125 — Ticket: Ensure byte-identical text (blocked by #124)
GLOSSARY: updated with "export format"
Done defined as: command runs, text identical, JSON parses, evidence in the PR
@Architect review seams if needed
@ProjectControl update the board (keep spec and tickets separate)
```

### 2. Architecture (if needed)

**Prompt for Architect:**
```
@Architect Review the design for #123/#124. Suggested test seams? Any ADR needed?
```

### 3. Implementation

**Prompt for Implementer (in the channel or private thread):**
```
@Implementer Pick up ticket #124.
Spec: #123
Agreed seams: [list from Architect or Spec]
Done = real evidence (command + text and JSON output)
Use TDD + prove-it-works. Branch ai/124-json-export
Open a draft PR with evidence.
```

**Implementer handoff to Reviewer:**
```
## PR Ready
PR: #456 (draft)
Issue: #124
Evidence: [paste output or links to screenshots/video]
Tests: red-green on seams
CI: green
@Reviewer can review
@Observability confirm evidence
```

### 4. Independent Review

**Reviewer responds in the channel and on the PR:**
```
## Review #456
Standards: OK
Spec: OK (criteria met)
Evidence: verified (I ran the command)
Decision: Approve
@ProjectControl move to Done
```

### 5. Documentation and Close-out

**After merge:**
```
@Docs Update changelog and examples with the new --json flag
@ProjectControl confirm the issue is closed and the board is updated
```

## Effective Communication Patterns with AI Models

1. **Declare done explicitly**  
   Always say what counts as success with observable criteria and ask for evidence.

2. **Minimum sufficient context**  
   Link to issue/spec/ADR instead of pasting everything. Let the agent read.

3. **Structured handoffs**  
   Use the format with ## Handoff, links, blockers and @mentions. This creates traceability in the channel history.

4. **One agent per responsibility**  
   Do not ask the Implementer to write the spec. Do not ask the Reviewer to implement.

5. **Grill before coding**  
   If there is ambiguity, force resolution (Spec Writer or grilling) before implementing.

6. **Evidence > confidence**  
   "Tests passed" is not enough. Ask for the real artifact (command, UI, log).

7. **Independence**  
   Reviewer ≠ Implementer. Observability verifies what was declared.

8. **Memory and skills**  
   Each agent persists what it needs and uses the right skills (do not improvise the process).

## Supporting Routines

- Project Control: daily status in `#project-status`
- Observability: CI/evidence alerts
- Docs: post-merge update
- Spec Writer: scan for issues without a clear spec

These flows use OpenBot Channels for persistent and traceable collaboration, while GitHub remains the source of truth for code and work.
