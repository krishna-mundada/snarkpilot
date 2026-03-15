# SnarkPilot 🛩️

> Your AI pair programmer. With a personality disorder.

SnarkPilot is a Claude Code extension that lets you work with an AI coding assistant in character. Pick a pilot, get technically correct advice wrapped in wildly different energy.

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

## How It Works

SnarkPilot integrates with [Claude Code](https://claude.ai/claude-code) via output styles and session hooks.

### Pick a pilot for the session

Open Claude Code and run:

```
/config
```

Select **Output style** → choose your pilot. Takes effect next session.

### Set a persistent default for a project

Create a `.pilot` file in your project root:

```bash
echo "grumpy-senior" > .pilot
```

SnarkPilot's session hook will automatically load that pilot every time you open Claude Code in that directory.

### PR reviews via GitHub Actions

Add an `ANTHROPIC_API_KEY` secret to your repo, then add a `.snarkpilot.json`:

```json
{
  "pilot": "grumpy-senior"
}
```

Every PR will get reviewed in character and posted as a comment automatically.

---

## Installation

Clone this repo and copy the `.claude/` directory into your project (or `~/.claude/` for global use):

```bash
# Project-level
cp -r .claude/output-styles /your-project/.claude/
cp -r .claude/hooks /your-project/.claude/
cp .claude/settings.json /your-project/.claude/

# Global (available in all projects)
cp -r .claude/output-styles ~/.claude/
cp -r .claude/hooks ~/.claude/
# merge hooks into your existing ~/.claude/settings.json
```

---

## Stack

- **Integration:** Claude Code (output styles + hooks)
- **PR reviews:** GitHub Actions + Claude API

---

*Built with Claude Code. Reviewed by the Grumpy Senior. Shipped by the Hype Founder.*
