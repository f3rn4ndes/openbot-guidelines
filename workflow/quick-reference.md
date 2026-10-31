# Quick Reference: Who to Talk to and When

| What you want to do                  | Talk to             | Intent prompt (template)                                      |
|--------------------------------------|---------------------|---------------------------------------------------------------|
| Clarify a vague idea                 | Spec Writer         | “I want [X]. Success criteria: [observable]. Do grilling.”    |
| Turn conversation into spec + tickets| Spec Writer         | “Turn this into a parent Spec and child Tickets. Publish to GitHub.” |
| Decide design / seams / ADR          | Architect           | “Review the design for #N. Define seams and ADR if needed.”   |
| Implement a ticket                   | Implementer         | “Implement #N. Done = real evidence. Open draft PR.”          |
| Review a PR                          | Reviewer            | “Review PR #N. Standards + spec + evidence. Clear decision.”  |
| Update docs / changelog              | Docs                | “Update docs from #N / PR #M.”                                |
| See status / blockers                | Project Control     | “What is the board status? List In Progress and blocked.”     |
| Check CI and evidence                | Observability       | “Check open PRs. Alert missing evidence or failing CI.”       |

## Standard Intent Phrase

> “@[Agent] [objective]. Desired result: [observable].”

This is enough. The agent chooses the right skills (Matt + poteto) under the hood.
