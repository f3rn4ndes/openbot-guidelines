# Beginner Examples: Learning the OpenBot Development Team

This file contains concrete, step-by-step examples for someone learning how to use the OpenBot team.  
Each example shows **who to talk to**, **what to say**, and **what to expect**.

Start with Example 1. Do not skip the alignment phase.

---

## Example 1 — Small Feature: Add a `--json` flag to a CLI command

**Goal**: Learn the full cycle on a tiny, low-risk change.

### Step 1 — Alignment (talk to Spec Writer)

In the `#spec-to-impl` channel:

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

**What you should see**: Spec Writer asks clarifying questions if needed, then confirms the criteria.

### Step 2 — Turn it into tickets

```
@SpecWriter Turn the alignment above into a spec and tickets on GitHub.
Done = at least one ticket with observable acceptance criteria and no open questions.
```

**Expected**: Spec Writer creates an issue and posts a handoff in the channel with the issue number.

### Step 3 — Implementation

```
@Implementer Implement the ticket created by Spec Writer (look for the handoff).
Done = real evidence: run the command with and without --json and paste the outputs.
Open a draft PR.
```

**Expected**: Implementer opens a draft PR with evidence in the body.

### Step 4 — Review

```
@Reviewer Review the PR that Implementer just opened.
Check that the evidence is real and that the acceptance criteria are met.
Give a clear decision.
```

### Step 5 — Lessons (optional but recommended the first few times)

```
@Implementer Reflect on this ticket.
Was anything unclear? What would make the next similar ticket smoother?
Present Accepted / Rejected / Backlog.
```

---

## Example 2 — Bug Fix: A command fails when a file is missing

**Goal**: Practice the bug-fix path and root-cause thinking.

### Step 1 — Alignment

```
@SpecWriter There is a bug: when the config file is missing, the tool crashes with a stack trace instead of a clear error message.

Users: anyone running the tool for the first time.
Observable success criteria:
- Missing config file produces a clear, actionable error message
- Exit code is non-zero
- The same behavior is shown with a real command run (paste the output)

Do grilling if needed.
```

### Step 2 — Tickets

```
@SpecWriter Turn this into a bug ticket with a clear reproduction and expected behavior.
```

### Step 3 — Implementation

```
@Implementer Implement the bug ticket.
First reproduce the failure with a real command.
Then fix it.
Done = evidence of the failure before and the clear error after.
```

### Step 4 — Review

```
@Reviewer Review the bug-fix PR.
Confirm that the reproduction evidence exists and that the new error message is clear.
```

---

## Example 3 — Asking for Status (anytime)

You can do this at any moment, even before starting work:

```
@ProjectControl What is the current status of the board?
List anything In Progress, In Review, or blocked, with links.
```

---

## Example 4 — Checking Evidence (anytime)

```
@Observability Are there any open PRs that are missing real evidence or have failing CI?
```

---

## Example 5 — Updating Documentation (after a merge)

```
@Docs The PR for the --json flag was merged.
Update the changelog and the command examples so they mention the new flag.
```

---

## Tips for Beginners

1. **Always start with Spec Writer** when the idea is not fully clear.
2. **Copy the prompts** above and adapt the details. Do not write loose prompts like “add the json thing”.
3. **Look at the handoff** the agent posts in the channel. That is your signal that the phase is done.
4. **Ask for a reflection** after the first few tickets. It helps you and the agents improve.
5. **Use the channels**. Do not send everything as private messages.
6. If an agent does something unexpected, reply in the channel with a correction and then ask for a reflection.

---

## What “Good” Looks Like

- Spec Writer ends with a handoff that has issue numbers.
- Implementer ends with a draft PR that contains pasted command output (or screenshot).
- Reviewer ends with a clear Approve or Request changes.
- Project Control can tell you the status without you hunting through GitHub.

Practice with Example 1 until the cycle feels natural. Then try Example 2.
