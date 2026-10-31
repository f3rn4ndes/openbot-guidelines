# Didactic Guide: From Zero to a Development Team with OpenBot

This guide is written to be followed step by step.  
You do not need to know Matt Pocock’s or poteto’s skills in advance.  
The agents use those skills under the hood. You only need to install OpenBot, create the agents, and talk to them with clear intent.

---

## 1. What is OpenBot

OpenBot is a local-first workspace for persistent AI agents.  
Each agent has its own workspace, memory, skills, routines, and history.  
They can collaborate in Channels and work in parallel.

Official site: https://openbot.run  
Repository: https://github.com/nightly-labs/openbot

---

## 2. Installation

### macOS
1. Download the latest `.dmg` from https://github.com/nightly-labs/openbot/releases
2. Drag OpenBot to the Applications folder.
3. Open the app.

### Windows
1. Download the latest `.exe`.
2. Run the installer.
3. If SmartScreen warns, click “More info” → confirm the publisher (SYNTHETIFY LABS) → “Run anyway”.

### Linux
1. Download the AppImage matching your processor (`uname -m`: x86_64 or aarch64).
2. Make it executable: `chmod +x OpenBot-*.AppImage`
3. Run: `./OpenBot-*.AppImage`

After opening, connect a provider (Claude, Codex/ChatGPT, Grok, Gemini, etc.).

---

## 3. Basic OpenBot concepts

- **Agent**: has persona/instructions, memory, skills, and routines.
- **Channel**: shared conversation between you and multiple agents.
- **Skill**: reusable instruction (SKILL.md format).
- **Routine**: scheduled or event-triggered task.
- **Agent workspace**: local folder where the agent works.

---

## 4. Creating the development team

You will create 7 agents. Start with the essentials and add the others later.

### Step 1 — Create the agents

In OpenBot, create one agent for each role below.  
Paste the instructions from the corresponding file (in the `roles/` folder) into the agent’s persona/instructions field.

| Agent              | Instructions file              | Priority |
|--------------------|--------------------------------|----------|
| Spec Writer        | roles/spec-writer.md           | High     |
| Architect          | roles/architect.md             | Medium   |
| Implementer        | roles/implementer.md           | High     |
| Reviewer           | roles/reviewer.md              | High     |
| Docs               | roles/docs.md                  | Medium   |
| Project Control    | roles/project-control.md       | High     |
| Observability      | roles/observability.md         | Medium   |

### Step 2 — Install recommended skills

From the OpenBot Marketplace or manually, install (when available):

- pstack / poteto-mode (poteto)
- Matt Pocock skills (grill-me, to-spec, to-tickets, tdd, code-review, retro, etc.)

Not all are required on day one. The agents already have instructions that cover the process.

### Step 3 — Create Channels

Create the following channels and add the relevant agents:

- `#spec-to-impl` → Spec Writer, Architect, Implementer, Project Control
- `#impl-review` → Implementer, Reviewer, Observability
- `#project-status` → Project Control + general reading
- `#docs` → Docs + Spec Writer / Architect
- `#observability` → Observability + Reviewer
- `#lessons` → All (optional, for reflections)

### Step 4 — Connect GitHub

Install the GitHub plugin/MCP on the agents that need it (Spec Writer, Implementer, Reviewer, Project Control).

---

## 5. How to interact with the team (full cycle)

### Practical example: add JSON export

#### 1. Alignment
In the `#spec-to-impl` channel:

```
@SpecWriter I want to add JSON export to the report command.

Users: developers who consume the output in pipelines.
Current problem: text only, hard to parse.
Observable success criteria:
- Text remains identical
- Valid and parseable JSON
- Both running against the sample project

Do grilling if needed. Do not write code.
```

#### 2. Spec + Tickets
```
@SpecWriter Turn the previous conversation into a spec and tickets.
Publish to GitHub.
Done = issues with observable acceptance criteria.
```

#### 3. Architecture (if needed)
```
@Architect Review the design for #123. Define test seams.
```

#### 4. Implementation
```
@Implementer Implement ticket #124.
Spec: #123
Done = real evidence (command run + text and JSON output).
Open a draft PR with Closes #124 and the evidence in the body.
```

#### 5. Review
```
@Reviewer Review PR #456.
Check Standards + fidelity to the spec + real evidence.
Decision: Approve or Request changes.
```

#### 6. Documentation
```
@Docs Update changelog and examples with the new --json flag.
```

#### 7. Lessons learned (when there was a correction)
```
@Implementer Reflect on the session for ticket #124.
Present Accepted / Rejected / Backlog and wait for approval.
```

Then record it in `LESSONS.md`.

---

## 6. Recommended routines

- **Project Control**: daily status in `#project-status`.
- **Observability**: periodic check of CI and evidence.
- **Docs**: post-merge update.
- **All**: weekly reflection or after complex tickets.

---

## 7. Communication principles

- Talk to the **agent that owns the phase**.
- Declare the **desired result** (observable).
- Avoid loose prompts.
- Prefer channels over private messages.
- Ask for reflection when there is a correction.

---

## 8. Next steps

1. Create the essential agents (Spec Writer, Implementer, Reviewer, Project Control).
2. Create the main channels.
3. Run a small ticket end-to-end.
4. Adjust memories and routines based on the result.
5. Keep `LESSONS.md` updated.

---

## Files in this repository

- `roles/` — detailed instructions for each agent
- `workflow/` — usage guides and flows
- `analysis/` — analyses of poteto and Matt Pocock
- `LESSONS.md` — lessons learned template
- `didactic-guide.md` — this file

Start with `didactic-guide.md` and `workflow/detailed-usage-guide.md`.
