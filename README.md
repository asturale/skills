# skills

Small Claude Code skills that add discipline without adding ceremony.

| Skill | Triggers | What it does |
|---|---|---|
| [`ponytail-proof`](skills/ponytail-proof/SKILL.md) | automatically: bugs, bulk/irreversible actions, before "done" | Evidence before claims, a red-first loop for bugs, one-item-first for bulk changes |
| [`secure-defaults`](skills/secure-defaults/SKILL.md) | new services, public repos, releases | Checklist for *missing* hardening: closed by default, sessions, proxy, secrets, temp files, containers |
| [`grill`](skills/grill/SKILL.md) | only when you type `grill` / `/grill` | Questions your plan in short rounds, each with a recommended answer |

`ponytail-proof` is written as a companion to a minimal-change skill like ponytail, but stands on its own.

## Install

```sh
git clone https://github.com/asturale/skills.git
cp -r skills/skills/* ~/.claude/skills/
```

Start a new Claude Code session to load them. To make `ponytail-proof` load reliably, name it in your `CLAUDE.md`, for example: "Load `ponytail-proof` for every bug, before any bulk or irreversible action, and before reporting work as done." Without that line, a test session did not pick it up on its own.

## Credits

Ideas adapted from [obra/superpowers](https://github.com/obra/superpowers) (verification-before-completion, systematic-debugging) and [mattpocock/skills](https://github.com/mattpocock/skills) (diagnosing-bugs, grilling), both MIT.
