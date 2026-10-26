# SWE-Skills

A personal collection of nine software engineering skills for Codex. The
skills cover readable code, software design, focused implementation and
code review. Instructions and invocation examples are written in English.

## Skills

| Skill | Type | Purpose |
|---|---|---|
| [code-style](.agents/skills/code-style/SKILL.md) | Readability and documentation | Improve formatting, names, API contracts, useful comments and verified usage examples. |
| [software-design](.agents/skills/software-design/SKILL.md) | Architecture | Clarify module responsibilities, interfaces, resource ownership and lifecycle contracts. |
| [karpathy-guidelines](.agents/skills/karpathy-guidelines/SKILL.md) | Implementation discipline | Make focused changes, expose assumptions, avoid unnecessary complexity and verify outcomes. |
| [ponytail](.agents/skills/ponytail/SKILL.md) | Implementation workflow | Make the smallest complete change, with `lite`, `full` and `ultra` modes. |
| [ponytail-review](.agents/skills/ponytail-review/SKILL.md) | Change review | Review a diff and connected code for concrete bugs, risks and unnecessary complexity. |
| [ponytail-audit](.agents/skills/ponytail-audit/SKILL.md) | Repository audit | Inspect a codebase and rank findings; produces a report without editing code. |
| [ponytail-debt](.agents/skills/ponytail-debt/SKILL.md) | Maintenance report | Collect deliberate `shortcut:` and older `ponytail:` comments into a debt ledger. |
| [ponytail-gain](.agents/skills/ponytail-gain/SKILL.md) | Benchmark reference | Show published upstream Ponytail results, not measured savings for the current project. |
| [ponytail-help](.agents/skills/ponytail-help/SKILL.md) | Usage reference | Explain Ponytail skills, invocation names and mode choices. |

## Layout

```text
.agents/
  skills/
    code-style/
    software-design/
    karpathy-guidelines/
    ponytail/
    ponytail-review/
    ponytail-audit/
    ponytail-debt/
    ponytail-gain/
    ponytail-help/
```

Each directory contains `SKILL.md` and `agents/openai.yaml`. Supporting
references and existing license files are kept with their owning skills.
This repository contains standalone skills, without plugin hooks or MCP
servers.

## Install

Git is the only tool needed to copy these instructions. Run the following
commands in a Linux, macOS or WSL shell.

Clone the collection:

```bash
mkdir -p "$HOME/.local/share"
git clone https://github.com/yangluo23/SWE-Skills.git "$HOME/.local/share/SWE-Skills"
```

### In a project

From the target project's root:

```bash
mkdir -p .agents/skills
cp -R "$HOME/.local/share/SWE-Skills/.agents/skills/." .agents/skills/
```

To install only one skill, copy its entire directory:

```bash
mkdir -p .agents/skills
cp -R "$HOME/.local/share/SWE-Skills/.agents/skills/code-style" .agents/skills/
```

### For your user account

For skills available across projects:

```bash
mkdir -p "$HOME/.agents/skills"
cp -R "$HOME/.local/share/SWE-Skills/.agents/skills/." "$HOME/.agents/skills/"
```

Codex discovers repository skills in `.agents/skills/` and user skills in
`~/.agents/skills/`. Newly copied skills are detected automatically; restart
Codex if they do not appear. See the [official OpenAI skills guide](https://learn.chatgpt.com/docs/build-skills).

Choose a project or user location to avoid duplicate skill names. Copying
merges directories and replaces matching files. Keep whole skill directories
so references, metadata and license files remain available.

## Use

Invoke a skill by name in a Codex prompt:

```text
Use $code-style and $software-design to improve this module's readability
and interface documentation while preserving behavior.

Use $karpathy-guidelines to implement this change with minimal scope and
appropriate verification.

Use $ponytail-review to review the current diff and its callers.

Use $ponytail-audit to audit this repository without modifying files.
```

The report-only skills do not implement fixes. Installing this collection
does not run a review or start a persistent Ponytail mode; invoke the workflow
you want. `$ponytail-help` explains the Ponytail options.

## Update

```bash
git -C "$HOME/.local/share/SWE-Skills" pull --ff-only
```

Repeat the relevant copy command to update the installed skills. Copies do
not automatically track this repository or remove obsolete files. Inspect
local customizations before replacing them.

## Design references and acknowledgements

`code-style` and `software-design` are locally maintained skills informed by
established Python conventions and PyTorch documentation practices:

| Reference | Applies to | Principles adopted |
|---|---|---|
| [PEP 8 — Style Guide for Python Code](https://peps.python.org/pep-0008/) | `code-style` | Layout, naming, imports, comments and consistency with project conventions. |
| [PEP 257 — Docstring Conventions](https://peps.python.org/pep-0257/) | `code-style` | Concise summaries, docstring structure and documentation of public interfaces. |
| [PEP 20 — The Zen of Python](https://peps.python.org/pep-0020/) | `software-design` | Readability, explicit behavior, simplicity and understandable structure. |

These references explain the basis for the guidance; project-specific rules
and actual implementation contracts take precedence. The skills adapt these
ideas into local review workflows rather than reproduce an upstream style
guide or require PyTorch's documentation markup.

Examples in the skills are independently written and do not copy PyTorch
implementation code. This collection is not an official Python or PyTorch
project and is not endorsed or certified by their maintainers.

## Sources and licenses

- `code-style` and `software-design` are locally maintained skills.
- `karpathy-guidelines` is adapted from
  [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills/tree/2c606141936f1eeef17fa3043a72095b4765b9c2).
  Its original source metadata and MIT declaration remain in `SKILL.md`.
- The six Ponytail skills are adapted from
  [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail/tree/09c434ab73ca43761c5d8b0c5d341926c2fba710),
  version 5.3.2. Each retains its MIT license and pinned source metadata.

Local adaptations include Codex invocation metadata and project-oriented
workflow/documentation guidance. Preserve upstream notices when copying or
redistributing adapted skills.
