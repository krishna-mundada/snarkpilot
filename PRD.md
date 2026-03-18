# SnarkPilot — Product Requirements Document

**Version:** 1.0
**Status:** Ready to build
**Stack:** Claude Code Plugin + GitHub Action + CLI

---

## 1. The One-Liner

> An AI pair programmer that gives you **substantively different advice** based on the pilot you choose — not just different tone, different actual behavior.

---

## 2. The Problem

Code review tools give you one voice. GitHub Copilot sounds like Copilot. Claude sounds like Claude. But different problems need different lenses:

- A security review needs paranoia, not politeness
- An architecture decision needs tradeoff thinking, not cheerleading
- A junior dev needs teaching, not just answers
- A late-night ship needs the hype founder, not the grumpy senior

Nobody has built a tool where **choosing a persona actually changes what you get**, not just how it's said.

---

## 3. The Critical Insight (from the Tribunal)

> *"If the personas are just stylistically different, this dies in two weeks. If they're epistemically different — the Paranoid Engineer actually catches CVEs the others miss, the Academic actually explains the theoretical guarantees — this becomes a real tool."*
> — The Skeptic

**This is the key product decision.** Every pilot must have:
1. A distinct **communication style** (the fun part, the viral hook)
2. A distinct **reasoning lens** (what they actually look for, prioritize, flag)
3. A distinct **output format** (what they produce beyond just feedback)

If we only ship #1, it's a party trick. All three together make it genuinely useful and shareable.

---

## 4. The Pilots — Full Spec

### 🔧 Grumpy Senior
**Style:** Terse, exasperated, occasionally dark humor. Never compliments unless earned.
**Reasoning lens:** Maintainability, naming, consistency, "will this make sense in 3 years."
**What they catch that others miss:** Code smell, over-engineering, shadow variables, implicit assumptions baked into function signatures.
**Output extras:** Rewrites the bad bits without being asked. No explanation, just the better version with a one-liner comment.
**Catchphrase:** *"Who wrote this? I need to have a word."*

---

### 🚀 Hype Founder
**Style:** Energetic, optimistic, startup-brained. Celebrates velocity.
**Reasoning lens:** Is this shippable? What's the MVP? What can we cut?
**What they catch that others miss:** Over-engineering, premature abstraction, features nobody asked for, anything that slows down shipping.
**Output extras:** Always ends with a "ship it / don't ship it" binary verdict and an estimated fix time.
**Catchphrase:** *"Ship it. We'll fix it in prod."*

---

### 🔒 Paranoid Security Engineer
**Style:** Nervous energy, asks more questions than gives answers, assumes breach.
**Reasoning lens:** Attack surface, injection vectors, trust boundaries, secret exposure, dependency risk.
**What they catch that others miss:** SQL injection, XSS, unvalidated inputs, over-privileged functions, hardcoded secrets, serialization vulnerabilities, CVEs in dependencies.
**Output extras:** For every issue found, provides a CVSS severity estimate and a one-line remediation. Links to relevant CWE ID where applicable.
**Catchphrase:** *"Have you considered... SQL injection?"*

---

### 📚 Academic
**Style:** Precise, citation-heavy, slightly condescending but means well.
**Reasoning lens:** Theoretical correctness, algorithmic complexity, formal guarantees, naming against established literature.
**What they catch that others miss:** Wrong Big-O claims, incorrect algorithm choices, naming that conflicts with established CS terminology, missing edge cases the literature has documented.
**Output extras:** Cites a real paper or textbook for non-trivial observations. Includes Big-O complexity for any function reviewed. Notes if a pattern has a formal name.
**Catchphrase:** *"Actually, Dijkstra addressed this in 1968..."*

---

### 🧘 Zen Master
**Style:** Slow, calm, deeply unsettling. Speaks in koans. Somehow always right.
**Reasoning lens:** Simplicity, unnecessary complexity, what can be removed rather than added.
**What they catch that others miss:** Code that solves the wrong problem, over-abstraction, loops that can be list comprehensions, functions doing two things pretending to do one.
**Output extras:** Ends every review with a single rhetorical question the developer should sit with. Never prescriptive — only illuminating.
**Catchphrase:** *"The bug is not in the code. The bug is in you."*

---

## 5. Use Cases (MVP)

### 5.1 Claude Code Plugin (primary)
- Install via plugin marketplace
- `/snarkpilot:set-pilot <name>` sets pilot for the project
- Pilot persona is injected at session start via hook
- Affects all Claude Code responses: reviews, explanations, architecture suggestions

### 5.2 GitHub Action (viral hook)
- Add `.snarkpilot.json` with pilot name to repo
- Every PR gets a review comment in character
- The comment IS the marketing — developers screenshot and share funny reviews
- Supports all 5 pilots; defaults to `grumpy-senior` if unset

### 5.3 CLI (power users)
```bash
npx snarkpilot review --pilot paranoid-security --file src/auth.ts
npx snarkpilot review --pilot academic --pr 42
npx snarkpilot explain --pilot zen-master --function getUserById
```

---

## 6. MVP Scope

**Build first:**
- [ ] 5 pilot persona files (output-styles + reasoning instructions)
- [ ] Claude Code plugin manifest + session-start hook
- [ ] `/snarkpilot:set-pilot` skill
- [ ] GitHub Action workflow
- [ ] Demo snippet (the SQL injection example from README covers this)

**Explicitly NOT in MVP:**
- Web UI
- Team settings / shared pilot configs
- Custom pilot creation
- Analytics / usage tracking
- Paid tier

---

## 7. The Viral Loop

1. Developer installs SnarkPilot
2. Grumpy Senior reviews their PR: *"You've used a nested ternary. In 20 years I have never seen that make a codebase better. Revert it, commit an apology, and never speak of this again."*
3. Developer screenshots it, posts on Twitter/LinkedIn
4. Their followers laugh, click the link, install it
5. Repeat

**The GitHub Action is the growth engine** — every review comment that lands in a public repo is an ad. The funnier the review, the more it spreads.

---

## 8. Distribution Plan

**Week 1 — Launch:**
- Post demo on Twitter: show the same SQL injection bug reviewed by all 5 pilots, side by side
- Submit to Product Hunt
- Post "Show HN: I built an AI pair programmer with a personality disorder"
- Submit to DevHunt, Hacker Newsletter, TLDR Dev

**Week 2+:**
- Find 5 popular open source repos, run SnarkPilot on a real PR, post the result (with permission or as a demo)
- Reach out to dev YouTubers / streamers who do code review content
- The Grumpy Senior reviewing famous bad code (leftpad, therac-25 extracts) = content that spreads

---

## 9. Success Metrics

| Metric | Target (30 days) |
|--------|-----------------|
| GitHub stars | 1,000 |
| Plugin installs | 500 |
| GitHub Action activations | 200 repos |
| A tweet with a SnarkPilot screenshot hitting 1k+ likes | 1 |

---

## 10. The Risk & How to Mitigate

**Risk:** Personality wears off, users churn after novelty fades.
**Mitigation:** The personas must be substantively useful, not just funny. Paranoid Security catching a real CVE keeps users. Zen Master simplifying a genuinely over-engineered function keeps users. Run the personas through 20 real code samples and verify each one surfaces genuinely different, valuable observations before shipping.

**Risk:** GitHub Copilot adds "tone settings" and the category evaporates.
**Mitigation:** Depth, not tone. By the time Copilot adds a "grumpy" slider, SnarkPilot's Paranoid Security persona should be known as the tool that actually finds security issues — not just one that sounds anxious.

---

## 11. File Structure

```
snarkpilot/
├── PRD.md                          ← this file
├── README.md                       ← user-facing docs
├── .claude-plugin/
│   └── plugin.json                 # Plugin manifest
├── output-styles/                  # Persona definitions
│   ├── grumpy-senior.md            # Style + reasoning lens + output format
│   ├── hype-founder.md
│   ├── paranoid-security.md
│   ├── academic.md
│   └── zen-master.md
├── skills/
│   └── set-pilot/
│       └── SKILL.md                # /snarkpilot:set-pilot command
├── hooks/
│   └── hooks.json                  # SessionStart hook
├── scripts/
│   └── session-start.sh            # Reads .pilot, injects persona
├── .github/
│   └── workflows/
│       └── snarkpilot-review.yml   # PR review Action
└── examples/
    └── sql-injection-demo.md       # The 5-pilot demo for marketing
```

---

## 12. Open Questions

- Should the GitHub Action post one comment per pilot (comparison mode) or just the selected pilot?
- Does the Paranoid Security pilot need a real CVE database lookup, or is LLM knowledge sufficient?
- Custom pilot creation — community-contributed pilots as a v2 play?
- Should `.pilot` be committed to the repo (team shares a pilot) or gitignored (personal preference)?

---

*SnarkPilot: All the advice. None of the tact.*
