---
name: karpathy-guidelines
description: Behavioral guidelines to reduce common LLM coding mistakes. Use when writing, reviewing, or refactoring code to avoid overcomplication, make surgical changes, surface assumptions, and define verifiable success criteria.
license: MIT
metadata:
  author: "forrestchang"
  source: "https://github.com/multica-ai/andrej-karpathy-skills"
  source-revision: "2c606141936f1eeef17fa3043a72095b4765b9c2"
---

# Karpathy Guidelines

Behavioral guidelines to reduce common LLM coding mistakes, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

Adapted for Codex from the upstream [karpathy-guidelines skill](https://github.com/multica-ai/andrej-karpathy-skills/blob/2c606141936f1eeef17fa3043a72095b4765b9c2/skills/karpathy-guidelines/SKILL.md). Keep clarification and verification proportionate to the task, and respect the user's existing decisions and authorization.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:

- State material assumptions explicitly. Resolve uncertainty from available project context first.
- If multiple interpretations materially change the outcome, surface the tradeoff. Use a reasonable stated assumption for minor, reversible choices.
- If a simpler approach exists, say so. Push back when warranted.
- Ask when missing information blocks a correct implementation. Continue independent work that does not depend on the answer.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Choose verification appropriate to the change. Use existing checks when sufficient; add tests when they meaningfully establish behavior or prevent regressions. Small, reversible edits may need only direct inspection.

For documentation work, verify the behavior shown by examples rather than
their wording or count. Keep examples proportional to likely caller mistakes;
do not change runtime behavior merely to make an example pass.

Transform tasks into verifiable goals:

- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```text
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Use the success criteria to work independently until the requested outcome is verified. Report what changed, the checks performed, and any unresolved limitations.
