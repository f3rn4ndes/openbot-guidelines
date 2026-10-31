# Didactic Guide: From Zero to a Development Team with OpenBot

This guide is written to be followed step by step, **by example**.  
Every step has a concrete action and a concrete expected result.  
Nothing is left implicit.

You do not need to know Matt Pocock’s or poteto’s skills in advance.  
The agents use those skills under the hood.

---

## 1. What is OpenBot

OpenBot is a local-first workspace for persistent AI agents.  
Each agent has its own workspace, memory, skills, routines, and history.  
They can collaborate in Channels and work in parallel.

Official site: https://openbot.run  
Repository: https://github.com/nightly-labs/openbot

---

## 2. Installation (by example)

### Example: Windows
1. Go to https://github.com/nightly-labs/openbot/releases
2. Download `OpenBot-Setup-x.y.z.exe`
3. Run the installer.
4. If SmartScreen appears: click “More info” → confirm publisher **SYNTHETIFY LABS** → “Run anyway”.
5. Open OpenBot.
6. Expected result: the main window opens and you see a place to connect a provider.

### Example: macOS
1. Download the `.dmg`.
2. Drag OpenBot to Applications.
3. Open it.
4. Expected result: same as above.

After opening, connect a provider (example: Claude, Codex, Grok).

---

## 3. Basic concepts (by example)

| Concept | Concrete example |
|---------|------------------|
| Agent | An agent named “Spec Writer” with instructions pasted from `roles/spec-writer.md` |
| Channel | A channel named `#spec-to-impl` that contains Spec Writer, Architect and Implementer |
| Private thread | The direct chat you open with Implementer to work on a long ticket |
| Workspace | The folder `~/OpenBot/Agents/spec-writer` where Spec Writer stores files |

---

## 3.5 Working with multiple projects (critical)

### How it works

You create each agent **once**.  
You create **one or more channels per project**.  
You add the **same agents** to the channels of every project.

Example with 2 projects:

| Project | Channels | Agents added |
|---------|----------|--------------|
| Project A | `#proj-a-spec-to-impl`, `#proj-a-impl-review` | Spec Writer, Implementer, Reviewer, Project Control |
| Project B | `#proj-b-spec-to-impl`, `#proj-b-impl-review` | The same Spec Writer, Implementer, Reviewer, Project Control |

### How the agent knows which project you mean

- When you post in `#proj-a-impl-review`, the Reviewer sees the history of **that channel**.
- When you post in `#proj-b-impl-review`, the same Reviewer sees the history of **that other channel**.
- The agent does **not** automatically mix the two histories.
- Its private memory is global, but the conversation context comes from the channel you are in.

### How to talk to the agent about a specific project

**Correct way (recommended):**
1. Open the channel of the project (example: `#proj-a-impl-review`).
2. Post:
   ```
   @Reviewer Review PR #456 for Project A.
   The ticket is #13. Check evidence and spec fidelity.
   ```

**Also correct (if you are in the private chat):**
```
@Reviewer This is for Project A (channel #proj-a-impl-review).
Review the PR that was opened there. Ticket #13.
```

### Mistakes that sabotage the team (do not do these)

| Mistake | Why it hurts | What to do instead |
|---------|--------------|--------------------|
| Putting all projects in one single channel | Agents lose track of which project a message belongs to. Context becomes noise. | Create at least one channel per project. |
| Asking an agent in a private chat about a project without naming the project or the channel | The agent may use the wrong context or invent details. | Always name the project and the channel. |
| Letting every agent reply to every message in a channel | Increases noise and context size. Agents start answering out of turn. | Mention only the agent that owns the phase (`@Reviewer`, `@Implementer`). |
| Creating a new Reviewer agent for every project | You lose the shared skills and memories, and you have to maintain many copies. | Reuse the same agent. Isolate by channel. |
| Pasting long code or logs in the channel | Pollutes the shared context for everyone. | Do heavy work in the private chat and only post the summary + link in the channel. |
| Forgetting to say which repository the work belongs to | Spec Writer or Implementer may open issues in the wrong repo. | In the first prompt of a new piece of work, write the full repo name (example: `f3rn4ndes/my-app`). |

### Concrete example of the correct flow for Project A

1. You are in channel `#proj-a-spec-to-impl`.
2. You post:
   ```
   @SpecWriter This is for repository f3rn4ndes/project-a.
   I want to add a --json flag...
   ```
3. Spec Writer creates the Spec and Tickets in that repository and posts the handoff in the same channel.
4. You (or Spec Writer) mention `@Implementer` in the same channel.
5. Implementer works and opens the PR.
6. You move to `#proj-a-impl-review` (or stay in the same channel) and post `@Reviewer Review PR #...`.

The Reviewer sees only the history of the channel you used. It does not get confused with Project B.

---

## 3.6 Modeling a project (new or existing) — by example

People often start with vague ideas, screenshots from the internet, or an existing codebase and no clear requirements.  
The goal of this phase is a **set of requirements** and a **domain model** (glossary + key decisions), not code.

### Who does this phase

**Main agent**: Spec Writer (with Architect when design decisions appear).  
Use Matt’s grilling style (or `/grill-with-docs` if the skills are installed).  
poteto’s approach is complementary: use the investigation playbook first on existing code, then design.

### Case 1 — New project (greenfield)

You have only ideas or screenshots.

**Exact prompt to Spec Writer (in the project channel):**
```
@SpecWriter This is a new project. Repository: f3rn4ndes/my-new-app.

I have some ideas and a few screenshots of similar apps.
I do not know the programming language or the design system yet.

Do grilling.
Help me decide:
- What the app actually needs to do (observable outcomes)
- Who the users are
- The main domain terms (start a GLOSSARY)
- Language and UI approach (only if they affect the requirements)

Do not write code. Do not pick a framework unless I ask.
Done = clear requirements + GLOSSARY draft + open decisions listed.
```

**Expected result**: Spec Writer asks closed questions, proposes terms for the GLOSSARY, and lists decisions that still need to be made (language, design system, etc.).

### Case 2 — Existing project (legacy)

You already have code and want to add something or understand it.

**Exact prompt to Spec Writer (or Architect):**
```
@SpecWriter This is for the existing repository f3rn4ndes/my-legacy-app.

I want to add [feature].
I have the current screens but I do not know how to write the requirements.

First investigate the relevant part of the code (do not change anything).
Then do grilling with me.
Produce:
- Updated GLOSSARY if new terms appear
- Observable acceptance criteria for the new feature
- Any ADR if a design decision is hard to reverse

Do not write production code.
```

**Expected result**: The agent reads the code, asks questions grounded in what exists, and produces requirements that fit the current system.

### What Matt and poteto do in this phase

| Approach | Best for | What it produces |
|----------|----------|------------------|
| Matt `/grill-me` or `/grill-with-docs` | Fuzzy ideas, new or existing projects | Interview that forces decisions + GLOSSARY + ADRs |
| poteto investigation playbook | Existing code you do not fully understand | Accurate mental model of current ownership and constraints |
| poteto `/architect` | Bigger design decisions after investigation | Structured design with options and trade-offs |

### Common starting points and how to phrase them

| What you have | How to start the prompt |
|---------------|-------------------------|
| Only screenshots from the internet | “I have screenshots of similar apps. Help me extract the real requirements from them. Do grilling.” |
| Screens but no requirements | “I have these screens. Help me write observable acceptance criteria for each. Do grilling.” |
| No idea of language or design system | “Do not pick a language or design system yet. First help me define what the app must do. List the decisions that depend on language later.” |
| Existing code, want to add a feature | “Investigate the relevant code first. Then grill me on the new feature so the requirements fit what already exists.” |

### Mistakes in the modeling phase

- Asking the agent to “just build it” before the requirements are clear.
- Letting the agent pick the language or framework in the first message.
- Skipping the GLOSSARY when new terms appear.
- Treating screenshots as requirements (they are only inspiration).

---

## 4. Creating the team — every step with a concrete example

### Step 1 — Create the Spec Writer agent (example)

1. Open OpenBot.
2. Click **+** or **New Agent**.
3. Name: `Spec Writer`
4. Open the file `roles/spec-writer.md` from this repository.
5. Copy the entire text under “Agent Instructions (system / persona)”.
6. Paste it into the agent’s Instructions field.
7. Save.
8. Open a private chat with Spec Writer and type exactly:
   ```
   What is your role?
   ```
9. Expected result: the agent answers that it turns intent into a parent Spec issue and child Ticket issues, and that it does not write production code.

Repeat the same procedure for the other agents using the files in the table:

| Agent              | File to copy from              |
|--------------------|--------------------------------|
| Architect          | roles/architect.md             |
| Implementer        | roles/implementer.md           |
| Reviewer           | roles/reviewer.md              |
| Docs               | roles/docs.md                  |
| Project Control    | roles/project-control.md       |
| Observability      | roles/observability.md         |

### Step 2 — Configure model (example for Spec Writer)

1. Open Spec Writer settings.
2. Model: choose a strong model (example: Claude 3.5 Sonnet or GPT-4o).
3. Reasoning effort: Medium.
4. Backup model: add a second model if the field exists.
5. Save.
6. Expected result: the agent settings show the chosen model and effort.

### Step 3 — Create the channel `#spec-to-impl` (example)

1. In the sidebar, click **Channels** → **+**.
2. Name: `spec-to-impl`
3. Add members: Spec Writer, Architect, Implementer, Project Control.
4. Post this exact message in the channel:
   ```
   Hello team. @SpecWriter confirm you can see this.
   ```
5. Expected result: Spec Writer replies in the channel.

### Step 4 — Connect GitHub and create the Project (example)

#### 4.1 Connect plugin (example for Spec Writer)
1. Open Spec Writer settings → Plugins / Integrations.
2. Connect GitHub and authorize.
3. In a private chat with Spec Writer, type:
   ```
   List the last 3 issues in the repository f3rn4ndes/openbot-guidelines
   ```
4. Expected result: the agent returns issue titles or says it can access the repository.

#### 4.2 Create labels (example)
In the GitHub repository, create these labels exactly:
- Name: `spec` — Color: `#1d76db`
- Name: `ticket` — Color: `#0e8a16`
- Name: `bug` — Color: `#d73a4a`
- Name: `needs-evidence` — Color: `#fbca04`
- Name: `blocked` — Color: `#e99695`

#### 4.3 Create the Project (example)
1. Repository → Projects → New project → Board.
2. Name: `Development Board`
3. Columns (create in this order):
   - Spec
   - Ready for Impl
   - In Progress
   - In Review
   - Done
4. Expected result: you see the board with those five columns.

#### 4.4 Create one view (example)
1. In the Project, click **+ New view**.
2. Name: `Specs progress`
3. Filter: Label = `spec`
4. Expected result: the view shows only issues labeled `spec`.

Create the other views the same way:
- `Ready work` → Label = `ticket` AND Status = Ready for Impl
- `In Review` → Status = In Review
- `Needs evidence` → Label = `needs-evidence`

### Step 5 — Configure a routine (example for Project Control)

1. Open Project Control settings → Routines.
2. Create a routine:
   - Name: `Daily status`
   - Schedule: every weekday at 09:00
   - Prompt: 
     ```
     Post a daily status in the channel #project-status.
     List Approved Specs, In Progress, In Review, Blocked, and Done since yesterday.
     ```
3. Save.
4. Expected result: the routine appears in the agent’s routine list.

---

## 5. Running a complete ticket — by example

### Example feature: add `--json` flag

#### 1. Alignment (exact prompt)
In channel `#spec-to-impl` post:

```
@SpecWriter I want to add a --json flag to the report command so that the output can be easily parsed by other tools.

Users: developers who run this in CI pipelines.
Current problem: the output is human-readable text only.
Observable success criteria:
- When --json is not passed, the output is identical to today
- When --json is passed, the output is valid JSON that can be parsed with jq
- Both modes work on the sample project in the repo

Do grilling if anything is unclear. Do not write code.
```

Expected result: Spec Writer asks questions or confirms the criteria.

#### 2. Create Spec + Tickets (exact prompt)
```
@SpecWriter Turn the alignment above into a parent Spec issue and child Ticket issues on GitHub.
Done = one Spec issue (label spec) and at least one Ticket issue (label ticket) with observable acceptance criteria and no open questions.
```

Expected result: Spec Writer posts a handoff with issue numbers (example: Spec #12, Ticket #13).

#### 3. Implementation (exact prompt)
```
@Implementer Implement ticket #13.
Spec: #12
Done = real evidence: run the command with and without --json and paste the outputs.
Open a draft PR with Closes #13 and the evidence in the body.
```

Expected result: Implementer opens a draft PR that contains pasted command output.

#### 4. Review (exact prompt)
```
@Reviewer Review the PR that Implementer just opened.
Check that the evidence is real and that the acceptance criteria are met.
Give a clear decision: Approve or Request changes.
```

Expected result: Reviewer posts a decision in the channel and on the PR.

---

## 6. Next steps

1. Create Spec Writer, Implementer, Reviewer and Project Control using the examples above.
2. Create the channel `#spec-to-impl` and add the agents.
3. Run the `--json` example end-to-end.
4. Look at the Project board views to see progress.

---

## Files in this repository

- `roles/` — instructions to paste into each agent
- `workflow/` — operational guides
- `examples/` — more examples
- `didactic-guide.md` — this file

Start here and follow the examples exactly.
