# GitHub Integration: Clear Progress from Approved Requirements to Done Work

## Goal

Make it possible to see project progress and to verify that what was approved was actually delivered. Avoid mixing requirements with implementation tickets.

## Core Separation

| Type              | Label   | Purpose                                      | Example title                  |
|-------------------|---------|----------------------------------------------|--------------------------------|
| Spec / Requirement| `spec`  | What was approved and why                    | Spec: JSON export for reports  |
| Ticket            | `ticket`| Vertical slice of work that implements part of a spec | Ticket: Add --json flag     |
| Bug               | `bug`   | Defect                                       | Bug: Crash on missing config  |

**Rule**: A spec issue never contains implementation tasks. It links to tickets. A ticket always links back to its spec.

## Recommended Issue Structure

### Spec Issue
```
Title: Spec: [short name]
Labels: spec
Body:
- Problem
- Users
- Observable acceptance criteria
- Links to tickets: #124, #125
```

### Ticket Issue
```
Title: Ticket: [short name]
Labels: ticket
Body:
- Parent spec: #123
- Acceptance criteria (copied or linked)
- Blocking edges
```

## Projects Board

Columns:
1. Spec (approved requirements)
2. Ready for Impl
3. In Progress
4. In Review
5. Done

Project Control moves items. Specs stay in the Spec column until all their tickets are Done.

## Traceability Rules

1. Every ticket links to its parent spec.
2. Every PR uses `Closes #ticket-number`.
3. PR body must contain real evidence.
4. After merge, the ticket closes automatically. The spec stays open until all tickets are done.
5. Observability flags any merged ticket whose PR lacks evidence.

## Daily Visibility

Project Control’s daily status shows:
- Approved specs and how many of their tickets are done
- What is in progress / in review / blocked
- What was completed since yesterday

This gives a clear view of progress without mixing requirements and tasks.

## Labels

- `spec`, `ticket`, `bug`, `docs`
- `needs-evidence`, `blocked`, `in-progress`, `in-review`

## Branch and PR Conventions

- Branch: `ai/<ticket-number>-<slug>`
- PR: `Closes #<ticket>` + evidence in the body
- No direct commits to main

## How this solves the previous problem

- Requirements stay in `spec` issues.
- User stories / implementation work stay in `ticket` issues.
- Progress is visible on the board and in the daily status.
- Evidence in the PR + Observability checks make it possible to confirm that approved work was actually done.

Project Control and Observability routines keep this structure healthy.
