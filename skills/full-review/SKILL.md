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

Once all 5 agents have returned, produce a single combined review in this format:

---

## SnarkPilot Full Review

### Consensus Issues
Issues raised by 2 or more pilots. These are the most important — multiple independent lenses flagged the same problem. For each, note which pilots flagged it.

### Specialist Finds
Issues raised by only one pilot that are still significant. Tag each with the pilot that found it (e.g. `[paranoid-security]`, `[academic]`). Include things the other pilots would not have caught.

### All Clear
Anything a pilot explicitly checked and found fine. Brief.

### Overall Verdict
One paragraph synthesizing all perspectives. What is the most important thing to fix? What is genuinely good?

**SHIP IT / DON'T SHIP IT** — make a call, weighed across all 5 perspectives.

---

Be direct. Do not repeat the same finding five times. If multiple pilots said the same thing in different words, merge them into one finding and note the consensus.
