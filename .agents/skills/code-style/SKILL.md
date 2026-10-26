---
name: code-style
description: >
  Write, review, or refactor code for human readability, including meaningful
  API docstrings and implementation comments. Use for formatting, naming,
  types, control flow, documentation, and error handling in code modules.
---

# Code Style

## Goal

Make code understandable to a developer who knows the language and domain
but is unfamiliar with the implementation. A reader should understand how
to use an interface and what must remain true when changing its code.

## Apply the Skill to the Requested Scope

- For a targeted edit, inspect the affected symbols and their callers.
  For a project-wide request, inventory maintained modules, exclude vendor
  and generated files, and work through the agreed modules by responsibility.
- Check formatting, names/control flow, API documentation, and explanatory
  comments separately. A formatter cannot assess the other three.
- For each reviewed module, read the relevant implementation and callers
  before adding documentation. Leave a file unchanged when it already meets
  the requirements; do not manufacture changes to increase coverage.
- For a large review, track which modules received formatting only, manual
  documentation review, or structural review. Report unreviewed areas rather
  than presenting a mechanical formatting pass as full compliance.

## 1. Follow the Project

- Read relevant project instructions, tool configuration, supported language
  versions, and nearby code before editing.
- Follow project rules first, then local conventions. Preserve compatibility.
- Read existing API documentation and documentation-tool configuration.
  Preserve its dialect (for example, Google-style or Sphinx/reStructuredText)
  instead of mixing syntaxes or converting the repository without a reason.
- Use the configured formatter and linter. Keep changes scoped to the task.
- Apply the Python defaults below where no project convention exists.

## 2. Formatting and Line Wrapping

- Use four spaces, 79-character code lines, and 72-character flowing
  comment/docstring prose as defaults.
- Continue inside `()`, `[]`, or `{}` with clear indentation; prefer this
  to backslash continuation.
- For expanded argument or collection lists, prefer one item per line
  with trailing commas, subject to the formatter.
- Prefer breaks before binary operators in new code.
- Keep quotes consistent and avoid unnecessary escaping.
- Keep short, readable expressions on one line. Do not shorten meaningful
  names, alter string contents, or change evaluation order to fit a limit.

## 3. Blank Lines

- Use two blank lines between top-level function or class definitions.
- Use one blank line between methods.
- Separate standard-library, third-party, and local import groups with
  one blank line.
- Within functions, separate conceptual stages sparingly. Keep related
  statements together; avoid decorative banners and excessive spacing.

## 4. Names and Types

- Name domain concepts and include units where ambiguity matters:
  `timeout_seconds`, `sample_count`, `normalized_weights`.
- Use `snake_case` for functions and variables, `CapWords` for classes,
  and `UPPER_CASE` for constants, respecting established API conventions.
- Use short mathematical names only where their meaning is clear locally.
- Add useful type annotations at interfaces and for shared data structures.
  Avoid repeating obvious types in comments.
- Use explicit parameters and meaningful keyword arguments. Use `Any`,
  generic mappings, or `**kwargs` when the contract genuinely requires
  flexibility, rather than to avoid defining it.

## 5. Control Flow and Helpers

- Prefer direct branches, loops, and simple comprehensions.
- Introduce intermediate variables when their names explain a concept
  or make evaluation steps easier to follow.
- Extract helpers when they name a meaningful operation, isolate complexity,
  or remove meaningful duplication.
- Avoid opaque one-liners, deeply nested expressions, and helpers that
  merely force readers to jump elsewhere.
- Do not enforce arbitrary function-length limits. Keep a coherent operation
  together when splitting it would make comprehension harder.

## 6. Comments

- Explain intent, constraints, tradeoffs, or surprising behavior.
  Avoid narrating obvious operations or inventing rationale.
- At a non-obvious branch, explain the condition being handled and the
  consequence of removing or changing it. At a state mutation, explain the
  invariant it protects when the assignment alone does not reveal it.
- Explain ordering requirements, retries, compatibility fallbacks, numerical
  tricks, synchronization and cleanup where those facts affect maintenance.
  Describe only guarantees supported by the code and tests.
- Keep explanatory comments accurate and use complete sentences.
- Place block comments above the relevant code, at matching indentation,
  with a `# ` prefix.
- Use inline comments sparingly: at least two spaces before `#`,
  then one space.
- Preserve the required syntax of tool directives such as `# noqa`.
- For substantial reasoning referenced across multiple locations, use
  a searchable `Note [Topic]`. State the invariant, why the straightforward
  alternative fails, and the affected paths. Keep the explanation beside
  its owning implementation and refer to it from dependent sites.
- For a type or lint suppression, preserve its directive and explain the
  specific limitation when it is not apparent. Do not silence a warning
  merely to make a readability refactor pass.
- Update or remove stale comments with the code. Do not add comment banners,
  line-by-line narration, or comment-density targets.

## 7. Docstrings

- Document public APIs and non-obvious internal contracts proportionately.
  A nontrivial API needs more than a restatement of its name.
- For a module, describe its responsibility and main entry points when they
  are not obvious. Mention important ownership or dependency boundaries;
  avoid maintaining a redundant catalog of every symbol.
- For a class, explain its role, owned state/resources, initialization and
  lifecycle. Document relevant public attributes and subclass obligations.
- For a function or method, explain the applicable parts of this contract:
  - Parameter meaning and relationships: units, shapes, ordering, valid
    combinations, identifier scope, and the meanings of empty or `None`.
  - Result meaning: structure, ordering, ownership, copies versus borrowed
    views, and when returned values remain valid.
  - Effects and timing: mutations, I/O, callbacks, legal call sequence,
    repeat-call behavior, lifetime and concurrency restrictions.
  - Failures and limitations: relevant exceptions, partial effects on
    failure, unsupported uses, and recovery obligations.
  Include only applicable facts; do not add empty sections or claim
  guarantees inferred solely from annotations or suggestive names.
- Use triple double quotes. Start function and method docstrings with
  a concise imperative summary ending in a period.
- Keep short docstrings on one line. For longer ones, separate the summary
  from details with one blank line and place closing quotes on their
  own line.
- Do not surround a one-line function docstring with blank lines.
  Separate a class docstring from its first method with one blank line.
- Follow the project's docstring format. Otherwise, use concise prose
  and `Args`, `Returns`, `Raises`, `Note`, or `Examples` sections when useful.
- Use warnings for concrete misuse consequences and notes for constraints
  or nuances. Use Sphinx roles/directives only in projects that support
  them. A PyTorch reference does not make Sphinx mandatory everywhere.
- Avoid repeating the signature, obvious types, or implementation details
  that callers do not need.
- Read [documentation examples](references/documentation-examples.md) when
  adding lifecycle documentation, richer docstrings or a design note.

### Choose and Verify Examples

- Review example needs separately from docstring presence. Prioritize public
  interfaces with call ordering, state publication, surprising input/output
  relationships, unknown values, or consequential failure behavior. Do not
  add examples to every symbol or target a numerical coverage percentage.
- Put a cross-method lifecycle example on the class or in one integration
  guide. Put a function-specific input/output example beside that function.
  Refer to the canonical example instead of repeating its setup in methods.
- Show the smallest complete, legal use and an observable outcome. Include
  a boundary case when it explains a likely mistake, such as unknown versus
  zero cost or saving versus deploying. Avoid incidental output and opaque
  identifier snapshots that make examples brittle without teaching behavior.
- Use real public names and explicit imports. Keep self-contained local
  examples executable; check them through the project's existing test runner
  or doctest support. Avoid mocks, private helpers or large fixtures merely
  to make a docstring executable.
- Label fragments requiring an existing runtime or external service as
  illustrative and state the setup, side effects and expected result.
  Do not silently run paid calls, downloads or live deployments to check docs.
- During a broad documentation review, record which important usage scenarios
  have verified examples, which refer to shared examples, and which remain
  illustrative or uncovered. Distinguish this from module-review coverage.

## 8. Errors and Refactoring

- For new behavior, validate relevant input constraints and provide errors
  that identify the problem, including expected and actual values when useful.
- Catch exceptions only to recover, translate, or add useful context.
  Preserve the original cause when translating an error.
- During style refactors, preserve defaults, validation, exceptions,
  ordering, laziness, numerical behavior, and side effects.
- Treat stricter validation and different failure behavior as functional
  changes, even when they appear cleaner.

## Verify and Report

- Read the diff for changed meaning in comments, defaults, errors, ordering
  and effects. Test behavior affected by extraction, renaming or relocation.
- Check that documentation agrees with implementation and actual callers.
  Do not fix a contradictory contract by silently changing runtime behavior.
- Validate referenced names, examples and documentation markup as relevant.
  Formatting/lint success and test success are useful evidence, not proof
  that all code is readable or fully documented.
- Report the scope actually reviewed, concrete documentation improvements,
  verification performed, and any remaining readability gaps.

## Reference

[PyTorch autograd/function.py](https://github.com/pytorch/pytorch/blob/main/torch/autograd/function.py)
is a useful reading example for documenting call constraints, related APIs,
limitations and intended use. Borrow the explanatory approach, while
following the target project's own conventions. The examples below are
independent examples, not copied PyTorch implementation code.

## Small Examples

### Wrapping a Long Expression

```python
total_loss = (
    reconstruction_loss
    + smoothness_weight * smoothness_loss
    + constraint_weight * constraint_loss
)
```

### Combining Layout, Documentation, and Useful Comments

```python
from torch import Tensor


def per_sample_mse(prediction: Tensor, target: Tensor) -> Tensor:
    """Return one mean squared error per sample.

    Both inputs must be real floating-point tensors with matching
    (batch, features) shapes. The result has shape (batch,).

    Raises:
        ValueError: If the inputs lack matching two-dimensional shapes.
    """
    if prediction.ndim != 2 or prediction.shape != target.shape:
        raise ValueError(
            "Expected matching (batch, features) shapes; "
            f"got prediction={tuple(prediction.shape)}, "
            f"target={tuple(target.shape)}"
        )

    # Keep the batch dimension so callers can weight samples individually.
    squared_error = (prediction - target).square()
    return squared_error.mean(dim=1)
```

