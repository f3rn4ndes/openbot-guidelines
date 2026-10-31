# Critical Analysis: poteto (pstack / poteto-mode) vs Matt Pocock Skills

## Overview

Both approaches attack the same central problem of AI-assisted development: **insufficient alignment + weak verification = wrong or incomplete code with high confidence**.

| Aspect | poteto / pstack | Matt Pocock Skills |
|--------|-----------------|--------------------|
| **Main focus** | Execution rigor, verification against the real artifact, named principles | Alignment before code (grilling), pipeline spec → tickets → TDD → review |
| **Entry** | `/poteto-mode` routes to playbooks | User-invoked skills (`/grill-with-docs`, `/to-spec`, `/to-tickets`, `/implement`) + model-invoked |
| **Verification** | Prove-it-works: evidence of real behavior (not "it compiles"), verification skills per app | TDD red-green on pre-agreed seams + two-axis code-review (Standards + Spec) |
| **Documentation** | Unslopped prose, name principles used | GLOSSARY.md, ADRs, specs published to the tracker |
| **Decomposition** | Playbooks (bug-fix, feature, shipping, investigation...) + sequence-verifiable-units | Tracer-bullet tickets with blocking edges + implement-spec with parallel subagents |
| **Independence** | Agent that verifies ≠ agent that wrote (shipping playbook) | Separate subagents for implement and review; parallel sub-agents in code-review |

## Strengths of poteto

1. **Prove-it-works as a non-negotiable principle**  
   "It compiles" is not evidence. Forces the agent to run the real artifact and show proof (command + output, screenshot, video, log). This drastically reduces the "slop" of false confidence.

2. **Playbooks copied verbatim**  
   `/poteto-mode` chooses the playbook and pastes the steps at the top of the TODO list. Reduces improvisation. Principles (laziness-protocol, fix-root-causes, sequence-verifiable-units, etc.) are indexed and must be cited when used.

3. **Verification skills per project**  
   5-job pattern: Launch → Doctor → Drive → Evidence → Cleanup. Makes verification repeatable and independent of the agent.

4. **Verification independence**  
   In shipping, the agent that judges is not the one that wrote. Excellent against self-deception.

## Weaknesses / Risks of poteto

- Can be verbose or too "mode-like" if applied to trivial tasks.
- Dependence on the agent actually reading and applying the principles (not just citing them).
- Less explicit emphasis on domain alignment *before* any code (although the investigation playbook helps).
- In multi-agent teams, centralized routing in poteto-mode can become a bottleneck if not well distributed.

## Strengths of Matt Pocock

1. **Grilling as product alignment**  
   Relentless interview until all decisions are resolved. With `grill-with-docs` it generates GLOSSARY.md and ADRs during the conversation. Solves problem #1: building the wrong thing.

2. **Clear and composable pipeline**  
   grill → to-spec → to-tickets → implement (TDD on agreed seams) → code-review.  
   `implement-spec` allows concurrency with subagents in worktrees + integration branch.

3. **Two axes in code-review**  
   Standards (style + Fowler smells) and Spec (fidelity to the originating issue/spec), in parallel sub-agents so they do not contaminate each other.

4. **Disciplined TDD**  
   Red-green (refactor removed in recent versions because agents rarely did it well). Pre-agreed seams in to-spec. Avoids tests that only pass if the internal implementation is exactly as imagined.

5. **Native integration with issue trackers**  
   Specs and tickets published to the tracker with native or textual blocking edges.

## Weaknesses / Risks of Matt Pocock

- TDD without explicit refactor can leave code "green but dirty" if the review is not rigorous.
- Grilling can be long; if the human does not respond with precision, alignment fails.
- Less emphasis on "prove on the real artifact" than poteto (depends more on tests + review).
- Per-repository setup (`/setup-matt-pocock-skills`) is necessary and easy to forget.

## Comparison: Modeling phase (new or existing projects)

| Aspect | Matt Pocock | poteto (pstack) |
|--------|-------------|-----------------|
| **Main style** | Structured interview (grilling) | Investigation + structured design |
| **Key skill / playbook** | `/grill-me`, `/grill-with-docs`, `domain-modeling` | Investigation playbook, `/architect`, `/how`, `/why` |
| **Best used when** | Intent is fuzzy, requirements are unclear, domain terms are not settled | Existing code you do not fully understand, larger design decisions |
| **What it forces** | Decisions and shared vocabulary | Accurate mental model of the current system + design options |
| **Main output** | GLOSSARY.md + ADRs + decisions in the conversation | Understanding of current code + design with trade-offs |
| **Approach** | Closed questions, one at a time, until no ambiguity remains | First reads the code (read-only), then proposes structure |
| **Risk if misused** | Can become long if there is no stopping criterion | Can skip the conversation and propose a design without aligning intent |

### How they complement each other

- **Matt first**: when the intention is not yet clear.  
  Use grilling to extract observable requirements and the domain vocabulary.

- **poteto after**: when code already exists or when the design decision is large.  
  Use investigation to understand what is already there, then `/architect` to structure the change.

### Practical sequence

1. Spec Writer (Matt style): “Do grilling. Help define what the app must do and start the GLOSSARY.”
2. Architect or Spec Writer (poteto style): “Investigate the relevant code. Then propose seams and an ADR if the decision is hard to reverse.”

---

## Recommended Synthesis for OpenBot

**Combining both is the most robust path:**

- **Alignment and specification phase**: Matt Pocock grilling + domain-modeling + to-spec + to-tickets. Generates GLOSSARY, ADRs, specs, and trackable tickets.
- **Implementation phase**: poteto-mode (or prove-it-works + sequence-verifiable-units principles) + Pocock’s TDD. Each ticket ends with real evidence.
- **Review phase**: Pocock’s code-review (two axes) + independent verification in poteto style (different agent, prove-it-works on the artifact).
- **Documentation and traceability**: Pocock’s ADRs/GLOSSARY + unslopped prose and principle citation from poteto.
- **Lessons learned**: poteto’s `/reflect` + Matt’s `/retro`.

This synthesis is reflected in the role definitions in this repository.
