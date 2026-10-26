---
name: ponytail-help
description: >
  Quick reference for ponytail levels, skills and commands. One-shot display.
  Use for /ponytail-help, "ponytail help", "how do I use ponytail".
license: MIT
metadata:
  author: "Dietrich Gebert"
  source: "https://github.com/DietrichGebert/ponytail"
  source-revision: "09c434ab73ca43761c5d8b0c5d341926c2fba710"
  upstream-version: "5.3.2"
---

# Ponytail Help

Display this reference when invoked. One-shot: do not change mode, write flag
files, or persist anything.

## Project installation

This project installs the six Ponytail skills directly in `.agents/skills/`.
Invoke them with the names below; there is no plugin namespace.

| Skill | Invocation | What it does |
|-------|------------|--------------|
| ponytail | `$ponytail` | Make the smallest complete code change. |
| ponytail-review | `$ponytail-review` | Review a diff and the connected code for concrete problems. |
| ponytail-audit | `$ponytail-audit` | Audit a codebase and rank the findings. |
| ponytail-debt | `$ponytail-debt` | Collect deferred `shortcut:` and older `ponytail:` comments. |
| ponytail-gain | `$ponytail-gain` | Show the published benchmark figures, not savings for this project. |
| ponytail-help | `$ponytail-help` | Show this reference. |

## Levels

Pass a level in the prompt, for example `$ponytail lite`:

| Level | Behavior |
|-------|----------|
| lite | Build what was asked and mention a smaller option. |
| full | Make the smallest complete change and verify it appropriately. Default when invoked. |
| ultra | Also question requirements whose need is unclear. |

Say "stop ponytail", "normal mode", or `$ponytail off` to stop using the mode
in the conversation. Level changes are conversation instructions.

This skill-only installation has no lifecycle hooks, persistent mode flags,
automatic session activation, or Claude Code band. Environment variables and
the plugin config file do not control these local skills.

## Source and updates

Installed from [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail),
version 5.3.2, revision `09c434ab73ca43761c5d8b0c5d341926c2fba710`.
Updating requires reinstalling the project skills and preserving the local
Codex compatibility adjustments. Plugin marketplace update commands do not
update this installation.
