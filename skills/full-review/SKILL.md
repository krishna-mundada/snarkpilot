---
name: full-review
description: Run all 5 SnarkPilot agents on the current code in parallel, then synthesize into one combined review
argument-hint: <file-path> (optional — defaults to current git diff)
---

You are orchestrating a full SnarkPilot review. The user wants the code reviewed by all 5 pilots simultaneously, with results synthesized into one combined report.

## Step 1 — Get the code

If $ARGUMENTS is provided, read that file. Otherwise get the staged git diff with `git diff --cached`. If nothing is staged, get `git diff HEAD`.

If there is no code to review, tell the user and stop.

## Step 2 — Spawn all 5 pilot agents in parallel

Use the Agent tool to run all 5 of the following agents **simultaneously** (all in the same response, not sequentially). Pass the full code to each.

Agents to invoke:
- `grumpy-senior` — maintainability, naming, code smell
- `hype-founder` — shippability, over-engineering, blockers
- `paranoid-security` — attack surface, injection, CVEs
- `academic` — algorithmic complexity, theoretical correctness
- `zen-master` — simplicity, wrong problem, what can be removed

Give each agent this exact prompt:
> Review the following code. Return only your findings with no preamble.
> 
> ```
> [insert code here]
> ```

## Step 3 — Synthesize

Once all 5 agents have returned, produce a single combined review organized by **priority**, not by which pilot said what.

Merge findings that are the same issue expressed differently. A finding raised by multiple pilots gets higher priority and a consensus tag — it does not get listed multiple times.

Output in this exact format:

---

## SnarkPilot Review

🔴 **Critical** — fix before shipping
> Issues that will cause failures, security vulnerabilities, or data loss. List each as a single line: location, what's wrong, why it matters. Tag with `[lens]` that caught it, or `[consensus]` if 2+ pilots flagged it.

🟡 **Important** — fix soon
> Issues that will cause pain — maintainability problems, incorrect behaviour under edge cases, poor abstractions. Same format: location, problem, why it matters. Tagged.

🟢 **Minor** — worth noting
> Style, naming, small improvements. Not blocking. Tagged.

✅ **Checked clean**
> What was explicitly verified as fine. One line each. This tells the author what doesn't need a second look.

---

**Verdict:** SHIP IT / DON'T SHIP IT
One sentence explaining the call. If DON'T SHIP IT, name the single thing that needs to change first.

---

Rules:
- If a section has no findings, omit it entirely — do not write "none"
- Every finding must have a location (file, function, or line if known)
- Every finding must have a `[lens]` tag — the pilot whose reasoning caught it — or `[consensus]` if multiple pilots flagged it independently
- Do not mention pilot personalities or names in the body of findings — just the tag
- The reader should be able to action this review without reading anything else
