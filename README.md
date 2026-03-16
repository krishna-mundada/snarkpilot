# SnarkPilot 🛩️

> Your AI pair programmer. With a personality disorder.

SnarkPilot is a Claude Code plugin that lets you work with an AI coding assistant in character. Pick a pilot, get technically correct advice wrapped in wildly different energy.

---

## The Pilots

| Pilot | Vibe | Catchphrase |
|-------|------|-------------|
| **grumpy-senior** | 20 years of scars, zero tolerance for nonsense | *"Who wrote this? I need to have a word."* |
| **hype-founder** | Moves fast, breaks things, raises Series A | *"Ship it. We'll fix it in prod."* |
| **paranoid-security** | Every input is an attack vector | *"Have you considered... SQL injection?"* |
| **academic** | Has a PhD. Cites papers. Always. | *"Actually, Dijkstra addressed this in 1968..."* |
| **zen-master** | Calm. Slow. Deeply unsettling. | *"The bug is not in the code. The bug is in you."* |

---

## Installation

```bash
claude plugin install github:krishna-mundada/snarkpilot --scope user
```

That's it. All five pilots appear in `/config` → **Output style** across all your projects.

---

## Usage

### Set a pilot from inside Claude Code

```
/snarkpilot:set-pilot grumpy-senior
```

Writes `.pilot` to the project root. Auto-loads that pilot at every session start.

### Or pick one per session

```
/config
```

Select **Output style** → choose your pilot. Takes effect next session.

### PR reviews via GitHub Actions

Add `ANTHROPIC_API_KEY` to your repo secrets, add `.snarkpilot.json`:

```json
{ "pilot": "grumpy-senior" }
```

Then copy `.github/workflows/snarkpilot-review.yml` from this repo into yours. Every PR gets reviewed in character.

---

## Demo

Same code. Five pilots.

```python
def get_user(id):
    query = "SELECT * FROM users WHERE id = " + str(id)
    return db.execute(query)
```

---

**grumpy-senior**
> Who wrote this? SQL concatenation in 2024. Not parameterized queries — *concatenation*. `id` also shadows a Python builtin, which is a special kind of wrong. Fix it: use parameterized queries, rename the argument, and add a return type annotation while you're in there feeling shame.

---

**hype-founder**
> Love the energy, love the velocity. Small note: this will absolutely get us hacked, which is not great for the Series B deck. Quick swap to parameterized queries and we are back to shipping. Five minutes, tops. Let's go.

---

**paranoid-security**
> Have you considered... SQL injection? Because I have. At length. `str(id)` is doing zero validation. A user passes `1 OR 1=1` and you've just handed them your entire users table. Parameterize this immediately. Also: what does `db.execute` return? Is that object getting serialized anywhere? I have more questions.

---

**academic**
> This exhibits a classic injection vulnerability first formally characterized by Halfond et al. in "A Classification of SQL Injection Attacks and Countermeasures" (2006). The remediation — parameterized queries — has been the consensus best practice for over two decades. Additionally, `id` as an identifier shadows the Python built-in `id()`, a naming collision Knuth would describe as an "unfortunate choice." The corrected implementation is O(1) with a constant factor determined by your database driver.

---

**zen-master**
> The query and the input are not separate things. You have made them one. This is the source of your problem, and also a metaphor. Separate them with a parameterized query. Then sit with why you merged them in the first place.

---

## Plugin Structure

```
snarkpilot/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── output-styles/           # One file per pilot
│   ├── grumpy-senior.md
│   ├── hype-founder.md
│   ├── paranoid-security.md
│   ├── academic.md
│   └── zen-master.md
├── skills/
│   └── set-pilot/
│       └── SKILL.md         # /snarkpilot:set-pilot command
├── hooks/
│   └── hooks.json           # SessionStart hook config
├── scripts/
│   └── session-start.sh     # Reads .pilot, injects persona
└── .github/
    └── workflows/
        └── snarkpilot-review.yml  # PR review Action
```

---

*Built with Claude Code. Reviewed by the Grumpy Senior. Shipped by the Hype Founder.*
