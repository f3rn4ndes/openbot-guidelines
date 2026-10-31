# Advanced Examples: External APIs and Equipment Control

These examples go beyond simple CLI flags. They show agents working with the outside world: HTTP APIs, sensors, and equipment control.

---

## Example A — Weather Alert Service (External API)

**Goal**: Build a small service that calls a weather API and sends an alert when rain is expected.

### Alignment

```
@SpecWriter I want a small script that checks the weather for a city and prints a clear alert if rain is expected in the next 6 hours.

Users: a person who runs it before leaving home.
Observable success criteria:
- Calls a real weather API (Open-Meteo or similar, no key required)
- Prints "No rain expected" or "Rain expected" with the probability
- Works with a real command that you run and paste the output
- Handles a network error with a clear message

Do grilling if needed. Do not write code yet.
```

### Tickets and Implementation

Follow the normal cycle. In the implementation prompt, require real evidence:

```
@Implementer Implement the ticket.
Done = real evidence: run the command against a real city and paste the output (both the no-rain and rain cases if possible).
Also show the error case (e.g., invalid city or simulated network failure).
```

---

## Example B — Controlling a Device via HTTP API (Home Automation style)

**Goal**: Turn a light on/off through a local HTTP API (simulated or real).

### Alignment

```
@SpecWriter I want a command that turns a light on or off by calling a local HTTP API.

Users: someone running a simple home-automation script.
Current problem: no easy way to control the light from the terminal.
Observable success criteria:
- `light on` calls POST /lights/1/on and prints success
- `light off` calls POST /lights/1/off and prints success
- Works against a real local server (or a mock you start)
- Evidence: command output + the HTTP response or server log

Do grilling if the API contract is unclear.
```

### Implementation note

Ask the Implementer to either:
- Start a tiny local mock server, or
- Document how to point it at a real device API

and to paste both the command and the evidence.

---

## Example C — Sensor Reading + Decision (IoT style)

**Goal**: Read a temperature sensor (or mock) and decide whether to turn a fan on.

### Alignment

```
@SpecWriter I want a script that reads a temperature value and decides whether to turn a fan on.

Users: a simple monitoring setup.
Observable success criteria:
- Reads temperature from a local endpoint or file (mock is fine)
- If temperature > 28°C, calls the fan-on API and prints the decision
- If temperature ≤ 28°C, does nothing and prints the decision
- Evidence includes the temperature value and the action taken

Do grilling if the thresholds or APIs are unclear.
```

---

## Tips for these advanced examples

1. Always require **real evidence** of the external call (command output + response or log).
2. Start with a mock server if you do not have real hardware. The pattern is the same.
3. Use the same agents and channels. Only the ticket content changes.
4. After the first successful external integration, ask for a reflection on what made the evidence clear.

These examples show that the same team process works when the software talks to the real world.
