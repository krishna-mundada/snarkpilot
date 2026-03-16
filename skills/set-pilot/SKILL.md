---
name: set-pilot
description: Set the active SnarkPilot persona for this project by writing to .pilot
argument-hint: grumpy-senior | hype-founder | paranoid-security | academic | zen-master
---

The user wants to set their SnarkPilot pilot to: $ARGUMENTS

Valid pilots are: grumpy-senior, hype-founder, paranoid-security, academic, zen-master

If $ARGUMENTS is empty or not provided, list the available pilots with a one-line description of each and ask the user to choose one.

If $ARGUMENTS is not a valid pilot name, tell the user and list the valid options.

If $ARGUMENTS is valid:
1. Use the Write tool to write the pilot name (and nothing else) to a file called `.pilot` in the current working directory.
2. Confirm in character as the chosen pilot — give a single in-character line that signals the pilot is now active.
3. Let the user know the pilot will auto-load next session via the SessionStart hook, and they can switch anytime with `/snarkpilot:set-pilot`.
