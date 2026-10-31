# Analysis: Agent Stops and Automation of PRs / GitHub

## 1. When the model stops and you have to ask it to continue

**Is it normal?**  
Yes. It is a known behavior of long-running agent turns, not a bug unique to OpenBot.

### Why it happens

- Models have context limits. When a thread gets long, the provider or OpenBot may compact or the model simply stops producing output.
- Some providers end the turn when they decide the task is “done” even if the agent’s plan is incomplete.
- Network hiccups, rate limits, or provider timeouts can interrupt a turn.
- OpenBot itself has FIFO queues, pause/resume, and crash-safe persistence, but a model turn can still end early.

### How OpenBot handles it

From the architecture and docs:

- **FIFO message queues** with pause, resume, cancellation, and crash-safe persistence.
- Channel tasks have explicit **Stop, Resume, Reassign** controls.
- When a turn is interrupted, you can resume or send a new message; the queue keeps the work.
- Routines and local scripts can wake an agent again.
- Backup models can take over if the primary hits a usage limit.

**Practical reality**: you will still sometimes need to say “continue” or “resume the previous task”. This is normal with current models. OpenBot reduces the pain (queue, resume, persistence) but does not eliminate the need for occasional human nudges on long tasks.

### Recommendations for this team

- Keep tickets small (tracer-bullet style). Shorter turns finish more reliably.
- Use the channel handoff pattern: the agent posts a clear “done / next” message so you know whether to nudge.
- For long implementation, prefer the Implementer to open a draft PR early and continue in a new turn if needed.
- Configure backup models so rate limits do not silently stop work.

---

## 2. Automation of closing PRs and updating GitHub

**Does poteto analyze every PR one by one?**  
Not in the way a human would. She has structured playbooks that an agent can run when asked, not a continuous background process that watches every PR forever.

### What poteto actually does

- **Babysit playbook**: when the user asks (“babysit this”, “get it green”), the agent owns the merge frontier, clears one PR at a time, watches CI, addresses comments, and stops where a human decision is needed. Modes: drive, background, threads-only, check.
- **Shipping playbook**: after babysit, independently verifies each PR (prove-it-works), then lands only the verified contiguous run. Can arm auto-complete on the frontier PR.
- These are **invoked**, not always-on. They are designed for stacks of PRs, not for a human to manually inspect every issue.

### What OpenBot adds

- **Routines**: scheduled tasks that can check status, post summaries, or run a check.
- **Local scripts API**: external tools can wake an agent and pass data.
- **Channels**: agents can be mentioned when a PR needs attention.
- Project Control and Observability agents can be given routines to poll the board and open PRs.

### Does full auto-close make sense?

**Partially yes, fully automatic no.**

| Level | What it does | Sensible? |
|-------|--------------|-----------|
| Status & alerts | Project Control / Observability post daily status, flag missing evidence or red CI | Yes — high value, low risk |
| Babysit on request | When you say “babysit #456”, Reviewer or a dedicated agent drives it to green | Yes — matches poteto |
| Auto-merge after verification | Only after independent prove-it-works + green CI + human or policy gate | Conditional — only if you trust the verification skill |
| Fully autonomous close of every PR | Agent decides and merges without any gate | Risky for most teams |

**Recommendation for this workflow**

1. **Do not** expect the agents to silently watch and close every PR by themselves.
2. **Do** give Project Control a routine that posts a status summary (In Progress, In Review, blocked).
3. **Do** give Observability a routine or on-demand check for missing evidence and red CI.
4. **Do** teach the Reviewer (or a shipping agent) the babysit/shipping pattern so that when you say “babysit this PR” or “ship the green ones”, it knows how to proceed safely.
5. Keep a human (or a very strict policy) as the final gate for merge on anything non-trivial.

This matches how poteto works at scale: verification is independent and structured, but the decision to land still has clear ownership and stops at human or policy boundaries.

### Summary

- Model stops are normal. OpenBot’s queue and resume help, but small tickets and clear handoffs reduce the need to nudge.
- Automation of GitHub status and alerts is very useful and should be done with routines.
- Full autonomous merging is possible in poteto style (babysit + independent verification + shipping) but should be explicit and gated, not silent background analysis of every PR.
