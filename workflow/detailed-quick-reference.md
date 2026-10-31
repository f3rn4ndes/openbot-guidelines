# Detailed Quick Reference: Who to Talk to and When

| What you want to do                            | Talk to             | Intent prompt (template)                                                                    | Skills used under the hood (reference)        |
|------------------------------------------------|---------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------|
| Clarify a vague idea / stress-test             | Spec Writer         | “I want [X]. Success criteria: [observable]. Do grilling. Do not write code.”              | grilling, grill-with-docs, domain-modeling   |
| Turn conversation into spec + tickets          | Spec Writer         | “Turn this into a parent Spec and child Tickets. Publish to GitHub. Done = issues with criteria.” | to-spec, to-tickets                           |
| Decide design / seams / ADR                    | Architect           | “Review the design for #N. Define seams and ADR if needed.”                                 | domain-modeling, codebase-design              |
| Implement a ticket                             | Implementer         | “Implement #N. Done = real evidence. Open draft PR with Closes #N.”                         | tdd, poteto-mode / prove-it-works             |
| Review a PR                                    | Reviewer            | “Review PR #N. Standards + spec + evidence. Clear decision.”                                | code-review, prove-it-works                   |
| Update docs / changelog                        | Docs                | “Update docs from #N / PR #M.”                                                              | unslopped prose, domain-modeling              |
| See status / blockers                          | Project Control     | “What is the board status? List In Progress and blocked.”                                   | GitHub tools, triage                          |
| Check CI and evidence                          | Observability       | “Check open PRs. Alert missing evidence or failing CI.”                                     | prove-it-works                                |
| Capture lessons learned                        | Implementer / Reviewer / any | “Reflect on #N. Errors corrected? What can be structural? Accepted/Rejected/Backlog.” | /reflect (poteto), /retro (Matt)              |

## Standard Intent Phrase

> “@[Agent] [objective]. Desired result: [observable].”

This is enough in most cases. The agent chooses the right skills under the hood.

## Lessons Learned Phrase

> “@[Agent] Reflect on [ticket/session]. Present Accepted / Rejected / Backlog and wait for approval.”
