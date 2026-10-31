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

- **Agent**: persistent teammate with persona/instructions, memory, skills, routines, and its own workspace. You create agents once and reuse them.
- **Private thread**: the direct conversation with one agent. It belongs to that agent.
- **Channel**: a shared conversation where several agents (and you) read and write. One task owner at a time. Handoffs stay visible.
- **Session / Task**: a unit of work that can be paused, resumed, or reassigned.
- **Skill**: reusable instruction (SKILL.md format).
- **Routine**: scheduled or event-triggered task.
- **Agent workspace**: local folder where the agent works (`~/OpenBot/Agents/<agent-id>`).

### Agents vs Channels (important for multiple projects)

You do **not** need to create a new set of agents for every project.

| Item | Create once or per project? | Recommendation |
|------|-----------------------------|----------------|
| Agents (Spec Writer, Implementer, etc.) | Once | Reuse the same agents across projects |
| Channels | Per project (or per phase) | Create channels named after the project |
| Workspaces | Per agent | Point the agent at the project folder when needed, or keep one workspace and switch context via the prompt |

**Practical pattern**:
- Create the agents once.
- For each project create channels such as `#projeto-x-spec-to-impl` and `#projeto-x-impl-review`.
- Mention the repository clearly in the first prompt of a new piece of work.
- The agents keep their skills and memories; the channel keeps the conversation history for that project.

---

## 4. Creating the development team (detailed procedures)

You will create 7 agents. Start with the essentials and add the others later.

### Step 1 — Create an agent (repeat for each role)

1. Open OpenBot.
2. In the left sidebar, look for the agents list or click **+** / **New Agent**.
3. Give the agent a name (example: `Spec Writer`).
4. In the agent settings, find the **Instructions** / **Persona** / **System prompt** field.
5. Open the corresponding file from this repository (see table below) and **copy the entire content** of the “Agent Instructions” section.
6. Paste it into the agent’s instructions field.
7. Save the agent.

Repeat for each agent:

| Agent              | Instructions file              | Priority |
|--------------------|--------------------------------|----------|
| Spec Writer        | roles/spec-writer.md           | High     |
| Architect          | roles/architect.md             | Medium   |
| Implementer        | roles/implementer.md           | High     |
| Reviewer           | roles/reviewer.md              | High     |
| Docs               | roles/docs.md                  | Medium   |
| Project Control    | roles/project-control.md       | High     |
| Observability      | roles/observability.md         | Medium   |

**Tip**: After pasting, open a private chat with the agent and ask “What is your role?” to confirm the instructions were loaded.

### Step 2 — Configure model and reasoning effort (recommended)

For each agent:
1. Open the agent’s settings.
2. Choose a model (see `guides/model-selection.md`).
3. Set reasoning effort (low / medium / high) according to the role.
4. If available, add a backup model for rate limits.

### Step 3 — Install recommended skills (optional on day one)

1. Open the OpenBot Marketplace (usually in the sidebar or settings).
2. Search for and install (when available):
   - pstack / poteto-mode
   - Matt Pocock skills (grill-me, to-spec, to-tickets, tdd, code-review, retro)
3. After installing, go back to the agent settings and attach the relevant skills to each agent.

Not all are required on day one. The agents already have instructions that cover the process.

### Step 4 — Create a Channel and add agents

1. In the sidebar, find **Channels** (or **Team Chat**).
2. Click **+** / **New Channel**.
3. Name it (example: `#spec-to-impl`).
4. Add the relevant agents to the channel (usually a button “Add members” or by mentioning them).
5. Repeat for the other channels:

- `#spec-to-impl` → Spec Writer, Architect, Implementer, Project Control
- `#impl-review` → Implementer, Reviewer, Observability
- `#project-status` → Project Control + general reading
- `#docs` → Docs + Spec Writer / Architect
- `#observability` → Observability + Reviewer
- `#lessons` → All (optional)

**Tip**: After creating a channel, post a simple message like “Hello team” and mention one agent (`@SpecWriter`) to confirm they can see it.

### Step 5 — Connect GitHub and set up the Project

#### 5.1 Connect the GitHub plugin
1. Open the agent settings for Spec Writer (repeat later for Implementer, Reviewer and Project Control).
2. Look for **Plugins**, **MCP**, or **Integrations**.
3. Connect the GitHub plugin and authorize it.
4. Confirm the agent can list issues or repositories.

#### 5.2 Create labels (once per repository)
In the GitHub repository, create these labels:
- `spec` (blue)
- `ticket` (green)
- `bug` (red)
- `docs` (gray)
- `needs-evidence` (orange)
- `blocked` (yellow)

#### 5.3 Create a GitHub Project (board)
1. In the repository, go to **Projects** → **New project**.
2. Choose **Board** layout.
3. Create the following columns:
   - Spec
   - Ready for Impl
   - In Progress
   - In Review
   - Done
4. Add the project to the repository.

#### 5.4 Configure useful views (for observability and decisions)
In the same Project, create these views:

| View name              | Filter / group by                          | Purpose |
|------------------------|--------------------------------------------|---------|
| Specs progress         | Label = `spec`                             | See approved requirements and how many tickets are done |
| Ready work             | Label = `ticket` AND status = Ready for Impl | What Implementer can pick next |
| In Review              | Status = In Review                         | What is waiting for Reviewer |
| Blocked                | Label = `blocked`                          | What needs a decision |
| Needs evidence         | Label = `needs-evidence`                   | PRs that Observability should flag |
| Approved vs Delivered  | Group by parent spec (or milestone)        | See if what was approved was actually done |

**Tip**: Project Control should look at “Specs progress” and “Approved vs Delivered” every morning. Observability should look at “Needs evidence” and “In Review”.

#### 5.5 What each agent configures / uses

| Agent              | GitHub configuration needed                          | What it does with GitHub |
|--------------------|------------------------------------------------------|--------------------------|
| Spec Writer        | Plugin connected, can create issues and labels       | Creates parent Spec issues and child Ticket issues |
| Implementer        | Plugin connected, can create branches and PRs        | Opens draft PRs with `Closes #N` and evidence |
| Reviewer           | Plugin connected, can comment and approve PRs        | Reviews PRs and checks evidence |
| Project Control    | Plugin connected, can move issues on the Project     | Maintains the board and posts daily status |
| Observability      | Plugin connected, can comment and add labels         | Flags missing evidence and failing CI |
| Architect / Docs   | Optional                                             | Links ADRs or updates docs |

### Step 6 — Configure routines (optional but useful)

1. Open the agent settings for Project Control.
2. Find **Routines** or **Scheduled tasks**.
3. Create a daily routine that posts a status summary in `#project-status`.
4. Do the same for Observability (evidence check).

---

## 5. How to interact with the team (full cycle)

### Practical example: add JSON export

#### 1. Alignment
Open the `#spec-to-impl` channel and post:

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

Wait for Spec Writer to reply (and ask clarifying questions if needed).

#### 2. Spec + Tickets
Still in the same channel:

```
@SpecWriter Turn the previous conversation into a parent Spec issue and child Ticket issues.
Publish to GitHub.
Done = one Spec issue (label spec) and linked Ticket issues (label ticket) with observable acceptance criteria.
```

Spec Writer should post a handoff with issue numbers.

#### 3. Architecture (if needed)
```
@Architect Review the design for #123. Define test seams.
```

#### 4. Implementation
You can stay in the channel or open a private chat with Implementer:

```
@Implementer Implement ticket #124.
Spec: #123
Done = real evidence (command run + text and JSON output).
Open a draft PR with Closes #124 and the evidence in the body.
```

#### 5. Review
In the channel or in `#impl-review`:

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
- Prefer channels over private messages for coordination.
- Use private chats for long implementation work.
- Ask for reflection when there is a correction.

---

## 8. Next steps

1. Create the essential agents (Spec Writer, Implementer, Reviewer, Project Control).
2. Create the main channels and add the agents.
3. Run a small ticket end-to-end following the example above.
4. Adjust memories and routines based on the result.
5. Keep `LESSONS.md` updated.

---

## Files in this repository

- `roles/` — detailed instructions for each agent
- `workflow/` — usage guides and flows
- `analysis/` — analyses of poteto and Matt Pocock
- `guides/` — model selection and other practical guides
- `examples/` — beginner and advanced examples
- `LESSONS.md` — lessons learned template
- `didactic-guide.md` — this file

Start with `didactic-guide.md` and `workflow/detailed-usage-guide.md`.
