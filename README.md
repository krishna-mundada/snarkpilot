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

That's it. The pilots will appear in `/config` → **Output style** across all your projects.

---

## Usage

### Pick a pilot for the session

```
/config
```

Select **Output style** → choose your pilot. Takes effect next session.

### Set a persistent default for a project

Create a `.pilot` file in your project root:

```bash
echo "grumpy-senior" > .pilot
```

SnarkPilot will automatically load that pilot every time you open Claude Code in that directory.

### PR reviews via GitHub Actions

Add `ANTHROPIC_API_KEY` to your repo secrets, then add `.snarkpilot.json`:

```json
{
  "pilot": "grumpy-senior"
}
```

And the workflow file — copy `.github/workflows/snarkpilot-review.yml` from this repo into yours. Every PR will get reviewed in character and posted as a comment.

---

## How It Works

SnarkPilot is a Claude Code plugin that bundles:

- **Output styles** — one per pilot, injected into Claude's system prompt. `keep-coding-instructions: true` so Claude's coding smarts stay intact; only the personality changes.
- **Session hook** — reads `.pilot` from your project root at session start and auto-loads the persona. No manual selection needed per session.

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
