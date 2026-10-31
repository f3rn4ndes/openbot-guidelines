# Prompt Patterns: Ready-to-Use Templates

Copy these templates and fill in the brackets.  
They are designed so that a beginner can get a good result on the first try.

## Alignment (Spec Writer)

```
@SpecWriter I want to [one-sentence goal].

Users: [who will use this].
Current problem: [what hurts today].
Observable success criteria:
- [criterion 1 that can be checked with a command or observation]
- [criterion 2]

Do grilling if anything is unclear. Do not write code.
```

## Turn Alignment into Tickets (Spec Writer)

```
@SpecWriter Turn the conversation above into a spec and tickets on GitHub.
Done = issues with observable acceptance criteria and clear blocking edges.
Post the handoff in this channel.
```

## Implementation (Implementer)

```
@Implementer Implement ticket #[NUMBER].
Spec: #[SPEC NUMBER]
Done = real evidence of the artifact (paste the command and its output).
Open a draft PR with Closes #[NUMBER] and the evidence in the body.
Do not implement anything beyond the ticket.
```

## Review (Reviewer)

```
@Reviewer Review PR #[NUMBER].
Check:
- Real evidence is present
- Acceptance criteria from the ticket are met
- Standards are followed
Decision: Approve or Request changes.
```

## Status (Project Control)

```
@ProjectControl What is the current status?
List In Progress, In Review, and blocked items with links.
```

## Evidence Check (Observability)

```
@Observability Check open PRs.
Alert any that are missing real evidence or have failing CI.
```

## Documentation Update (Docs)

```
@Docs Update the relevant docs and changelog from PR #[NUMBER] (merged).
Keep consistency with the GLOSSARY.
```

## Reflection / Lessons (any agent)

```
@Implementer Reflect on ticket #[NUMBER].
What was corrected or was harder than expected?
What can be made structural (lint, test, skill, checklist)?
Present Accepted / Rejected / Backlog and wait for approval.
```

---

## Quick Decision Tree

- Idea is vague → Spec Writer (Alignment template)
- Idea is clear but not yet tickets → Spec Writer (Tickets template)
- Ticket exists and is clear → Implementer
- PR is open → Reviewer
- You are lost → Project Control
- You want to know if things are healthy → Observability
- Something was merged → Docs
- Something went wrong or was corrected → Reflection template
