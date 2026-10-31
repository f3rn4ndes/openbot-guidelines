# Model Selection and Reasoning Effort for OpenBot Agents

## Why this matters

Not every agent needs the strongest model or the highest reasoning effort.  
High-reasoning models cost more tokens and take longer. Using them for simple tracking or documentation work wastes budget and slows the team.

OpenBot lets you set **model** and **reasoning effort** per agent.  
It also supports **backup models** (reserve models) that the agent can fall back to when the primary hits a usage limit or the provider returns an error.

## How to evaluate which model and effort an agent needs

### 1. Classify the agent’s work

| Agent role          | Typical work                        | Recommended starting point          | Reasoning effort |
|---------------------|-------------------------------------|-------------------------------------|------------------|
| Spec Writer         | Grilling, domain modeling, tickets  | Strong model (Claude Opus / GPT high) | Medium–High     |
| Architect           | Design decisions, seams, ADRs       | Strong model                        | Medium–High     |
| Implementer         | TDD, code, real evidence            | Strong coding model                 | Medium–High     |
| Reviewer            | Independent review, standards + spec| Strong model                        | Medium          |
| Project Control     | Status, board updates, reporting    | Fast / cheaper model                | Low             |
| Observability       | CI checks, evidence alerts          | Fast / cheaper model                | Low             |
| Docs                | Changelog, examples, GLOSSARY       | Medium model                        | Low–Medium      |

### 2. Start low and escalate

Research on reasoning effort shows that higher effort is not always better and can cost up to 40× more on easy tasks.  
Best practice: start with the lowest effort that works, then raise it only when the agent fails or produces incomplete results.

### 3. Use backup models

In the agent settings, configure backup models (automatic or ordered list).  
Typical pattern:
- Primary: strong model for the hard phases
- Backup: cheaper/faster model for when the primary is rate-limited

OpenBot keeps the agent’s workspace, thread, and identity when it switches models.

### 4. Measure in practice

- Run the same small ticket twice: once with low effort, once with medium/high.
- Compare: quality of the handoff, presence of real evidence, number of corrections needed, and (if visible) token usage.
- If low effort produces clear tickets and real evidence, keep it.
- If the agent skips grilling, invents requirements, or fails to show evidence, raise the effort or switch model.

## Practical recommendations for this team

| Agent             | Suggested primary          | Backup                  | Effort |
|-------------------|----------------------------|-------------------------|--------|
| Spec Writer       | Strongest available        | Medium model            | Medium |
| Architect         | Strongest available        | Medium model            | Medium |
| Implementer       | Best coding model          | Strong general model    | Medium |
| Reviewer          | Strong model               | Medium model            | Medium |
| Project Control   | Fast / cheap               | Any                     | Low    |
| Observability     | Fast / cheap               | Any                     | Low    |
| Docs              | Medium                     | Fast                    | Low    |

Adjust after the first few real tickets. The goal is the lowest cost that still produces observable, correct results.

## Where to configure in OpenBot

- Agent settings → Model picker → choose provider + model
- Same screen → Reasoning effort (Low / Medium / High / etc., depending on provider)
- Agent settings → Backup models → set automatic or ordered list

Start with the table above, run Example 1 from the beginner examples, and adjust based on what you see.
