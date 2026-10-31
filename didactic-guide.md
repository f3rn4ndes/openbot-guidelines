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
