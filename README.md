# OpenBot Development Team Setup

This directory contains the complete definition of a persistent agent team for software development in OpenBot (nightly-labs/openbot), from specification alignment to review, with documentation, project control, and observability.

## Files

### Analysis
- [analysis/poteto-matt-pocock.md](analysis/poteto-matt-pocock.md) — Critical analysis of poteto (pstack) and Matt Pocock Skills approaches, and how to synthesize them.
- [analysis/licoes-aprendidas.md](analysis/licoes-aprendidas.md) — How poteto and Matt handle lessons learned and error correction.

### Roles (each with instructions, memories, skills, routines, analysis, and GitHub integration)
- [roles/spec-writer.md](roles/spec-writer.md)
- [roles/architect.md](roles/architect.md)
- [roles/implementer.md](roles/implementer.md)
- [roles/reviewer.md](roles/reviewer.md)
- [roles/docs.md](roles/docs.md)
- [roles/project-control.md](roles/project-control.md)
- [roles/observability.md](roles/observability.md)

### Workflow
- [workflow/prompt-flows.md](workflow/prompt-flows.md) — Concrete prompt flows and Channel handoffs.
- [workflow/github-integration.md](workflow/github-integration.md) — How to use Issues, PRs, Branches, and Projects so you don't get lost.
- [workflow/guia-de-uso-detalhado.md](workflow/guia-de-uso-detalhado.md) — Detailed operational guide: who to talk to, when, and why.
- [workflow/referencia-rapida-detalhada.md](workflow/referencia-rapida-detalhada.md) — Quick reference table.

### Guides
- [guides/model-selection.md](guides/model-selection.md) — How to choose model and reasoning effort per agent, and how to use backup models.

### Examples (for learning)
- [examples/beginner-examples.md](examples/beginner-examples.md) — Step-by-step examples for learning the tool.
- [examples/prompt-patterns.md](examples/prompt-patterns.md) — Ready-to-use prompt templates.
- [examples/advanced-examples.md](examples/advanced-examples.md) — External APIs, sensors, and equipment control examples (with diagram descriptions).

## How to Use in OpenBot

1. Create an agent for each role (or start with Spec Writer, Implementer, Reviewer, and Project Control).
2. Paste the role instructions into the agent’s persona/instructions field.
3. Install the recommended skills (via Marketplace or locally). poteto-mode / pstack and Matt Pocock skills are especially relevant.
4. Configure model and reasoning effort per agent (see guides/model-selection.md). Use backup models for rate limits.
5. Configure routines as described.
6. Create the recommended Channels and add the agents.
7. Connect the GitHub plugin on the agents that need it.
8. Start with an intent prompt in the `#spec-to-impl` channel following the examples.

**Read first:**
- [guia-didatico.md](guia-didatico.md) — Complete didactic guide from installation to team usage.
- [examples/beginner-examples.md](examples/beginner-examples.md) — Step-by-step examples for learning the tool.
- [examples/advanced-examples.md](examples/advanced-examples.md) — External world examples.
- [guides/model-selection.md](guides/model-selection.md) — Model and reasoning effort choices.
- [workflow/guia-de-uso-detalhado.md](workflow/guia-de-uso-detalhado.md) — Detailed operational guide.
- [LESSONS.md](LESSONS.md) — Lessons learned template.

The agents use Matt Pocock and poteto skills under the hood (including `/reflect` and `/retro`). You only need to talk to the right agent, in the right phase, with clear intent.

## Core Communication Principle

> Explicitly declare what "done" means with observable criteria and require evidence of the real artifact. Align before coding. Keep independence between who implements and who reviews.

This combines Matt Pocock’s grilling and pipeline with poteto’s prove-it-works and independent verification.

## Suggested Next Steps

- Adapt the roles to your specific repository (branch conventions, CI, verification skill).
- Create a verification skill for your app (poteto pattern: launch/doctor/drive/evidence/cleanup).
- Run a pilot with a small ticket and adjust memories/routines based on the result.
- Use the Docs agent to keep GLOSSARY.md and ADRs alive from the start.

All files are ready to be copied or used as the basis for configuring agents in OpenBot.
