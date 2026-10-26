---
name: software-design
description: >
  Design, review, or evolve software architecture with clear responsibilities
  and documented behavioral contracts. Use for module boundaries, public
  APIs, dependencies, state/resource ownership, and extension protocols.
---

# Software Design

## Goal

Build the smallest sufficient design that meets the requirements and
remains easy to understand, debug, and change.

Treat these principles as decision guides. Follow project constraints
and resolve tradeoffs using concrete use cases.

## Apply the Skill to the Requested Scope

- For a focused change, trace affected callers, state and resource owners.
  For a project-wide request, inventory maintained modules, exclude vendor
  and generated artifacts, and review each included responsibility group.
- Separate observed problems from preferences. Explain a concrete cost to
  callers or maintainers before introducing a new boundary or abstraction.
- Review code, not just the directory tree, function lengths or docstrings.
  A long function may be coherent; a short function may hide costly I/O or
  a fragile lifecycle. Evaluate the actual responsibilities and invariants.
- Track reviewed areas on larger tasks. If review is incomplete, report the
  remaining areas; formatter success does not establish sound architecture.

## 1. Start with Callers and Constraints

- Read existing components and their callers before changing structure.
- Identify the common use cases, expected behavior, and relevant constraints:
  compatibility, latency, memory, concurrency, and execution environment.
- Keep routine usage straightforward while preserving access to the
  underlying capabilities.
- Introduce user-facing restrictions only when their benefits justify
  the cost to callers.
- Before moving code, identify who owns each decision, mutation, resource
  and failure path. Keep those owners visible after the change.

## 2. Choose Structure for a Concrete Reason

- Start with a function for stateless operations. Use a class when it
  represents a meaningful domain object or owns state, invariants,
  or a lifecycle.
- Introduce an abstraction when you can name the responsibility or
  concrete variation it captures.
- Prefer composition for interchangeable behavior. Use inheritance for
  meaningful subtype relationships or established extension protocols.
- Add registries, base classes, configuration systems, or plugin frameworks
  only for a specific need.
- Allow small duplication when similar-looking operations have different
  meanings or reasons to change.

## 3. Organize Around Responsibilities

- Group code that shares responsibilities, invariants, or reasons to change.
- Keep dependencies understandable and avoid unnecessary circular coupling.
- Separate input adaptation, computation, orchestration, and I/O when
  doing so clarifies ownership or supports independent changes.
- Avoid forcing every operation through a fixed set of layers.
- Reuse the language's ecosystem and established protocols before creating
  custom infrastructure.

## 4. Make API Boundaries Clear

- Distinguish public APIs, documented extension interfaces, and internals.
- Expose the capabilities callers need while keeping implementation details
  replaceable.
- Document a nontrivial interface's actual behavioral contract: required
  state, accepted input relationships, result ownership, observable effects,
  relevant failures and repeat-call behavior. Type signatures alone do not
  express these obligations.
- Treat public and documented subclass interfaces as compatibility
  commitments. Follow the project's policy for breaking changes.
- Prefer straightforward attributes for simple public data.
  Keep properties inexpensive and unsurprising; expose substantial
  computation or I/O through explicit methods.
- Keep convenient defaults, but make consequential choices configurable
  when callers have a real need to control them.
- Check the API through a minimal caller scenario. A lifecycle example
  should make construction, legal ordering, the publication point and
  cleanup ownership visible. Keep that example at the owning abstraction
  or integration guide rather than duplicating it across every method.
- If an example needs unexplained private calls or repeated setup workarounds,
  inspect the boundary before adding abstractions. Distinguish a real API
  usability issue from unavoidable external environment setup; document
  illustrative integration requirements honestly.

## 5. Make Important Behavior Discoverable

- Make significant dependencies, mutations, I/O, allocation, and device
  transfers visible through interfaces or documentation.
- Normalize flexible inputs at clear boundaries and use consistent
  representations internally.
- Give validation and compatibility adaptation a clear owner.
- Keep decisions requiring broader context at a layer that has that context.
- Report failures clearly. Make fallback behavior deliberate and observable
  when it changes correctness, cost, or execution semantics.
- Preserve useful convenience APIs when their behavior remains predictable
  and debuggable.

## 6. Define State and Resource Ownership

- Identify who creates, mutates, shares, and releases each resource.
- Use context managers, RAII, or `try/finally` for cleanup that must happen
  on both successful and exceptional exits.
- For temporary state changes, restore the previous state rather than
  assuming a default.
- Define nesting, reentrancy, and concurrency behavior where relevant.
- Distinguish owning a resource from borrowing one. State when a borrowed
  object or returned view becomes invalid and whether callers may retain it.
- For stateful components, describe legal transitions and which operations
  only stage changes versus making them visible. Identify the commit or
  publication point when partial state must not become externally visible.
- Separate ordinary exception cleanup, retry safety and crash recovery.
  Do not call an operation atomic, durable, idempotent or thread-safe unless
  the implementation supports that specific guarantee and failure model.
- Remove temporary hooks and callbacks when their lifetime ends.
- Keep cache ownership, invalidation, and retention policies explicit
  when caches affect behavior or resource usage.

## 7. Extend Through Narrow Contracts

- Add extension points for demonstrated or explicitly required variation.
- Define invocation timing, inputs, outputs, side effects, and lifecycle
  obligations.
- For callbacks and subclass hooks, document who invokes them, legal call
  order, owned versus borrowed arguments, permitted mutations, return-value
  correspondence and exception handling. Explain required overrides and
  defaults at the extension interface rather than at every call site.
- Separate stable mechanics from variable policy when multiple policies
  are actually needed.
- Reuse established framework extension mechanisms and lifecycle APIs.
  Avoid bypassing their invariants.
- Keep extension behavior traceable without requiring readers to navigate
  excessive indirection.

## 8. Optimize and Evolve Deliberately

- Choose suitable algorithms and data structures from the start.
- Meet explicit performance requirements while keeping interfaces usable.
- Justify additional implementation complexity with measurements or
  clear complexity reasoning.
- Keep optimized implementations behind understandable contracts when
  practical, with useful diagnostics.
- During structural refactoring, preserve observable behavior, including
  defaults, errors, ordering, laziness, and relevant numerical or gradient
  semantics.
- Keep architectural changes scoped. Explain the concrete benefit of
  each new boundary or abstraction.

## 9. Keep Design Rationale with Its Owner

- Put caller obligations and observable behavior in public API docstrings.
  Put implementation rationale beside the owning algorithm or state change.
  Use a short module or class explanation for relationships spanning methods.
- When a subtle invariant affects several paths, write one canonical
  `Note [Topic]` and reference it from those paths. Explain the invariant,
  the failure it prevents and the relevant coordination points.
- Document constraints that make an otherwise reasonable alternative wrong:
  ordering dependencies, compatibility, shared state, cancellation, numerical
  behavior or isolation. Avoid unsupported stories about past decisions.
- Reference the actual owning symbols and related contracts so readers can
  follow the mechanism. Keep those references current when code moves.
- Use an example or a compact state/ownership table when it clarifies a
  protocol. Do not create an architecture document for every small change.
- Improve boundaries when documentation reveals mixed responsibilities.
  Do not use lengthy comments to conceal confusing control flow, or split
  coherent code into forwarding helpers to satisfy a size target.

## Verify and Report

- Trace representative success and failure paths through changed boundaries.
  Verify cleanup, partial effects, state visibility and extension timing where
  those contracts apply. Use tests that exercise the relevant behavior.
- Check that documented guarantees match code and callers. Treat a change to
  call order, validation, errors, retries, serialization or deployment timing
  as a behavior change, even when the new design looks cleaner.
- Preserve observable interfaces and document ownership at the new location.
  Avoid maintaining two competing explanations of the same invariant.
- Report which modules received design review, why each structural change
  helps, how behavior was checked, and unresolved risks or unreviewed areas.

## Examples

### Distinguish Data Access from Computation

```python
sample_count = dataset.sample_count
statistics = dataset.compute_statistics()
```

Use attribute access for inexpensive data retrieval and an explicit
method for substantial computation.

### Restore the Previous State

```python
import logging
from collections.abc import Iterator
from contextlib import contextmanager


@contextmanager
def temporary_log_level(
    logger: logging.Logger,
    level: int,
) -> Iterator[None]:
    """Temporarily change a logger's level and restore it on exit."""
    previous_level = logger.level
    logger.setLevel(level)
    try:
        yield
    finally:
        logger.setLevel(previous_level)
```

Saving the entry state supports sequential nesting and restoration after
exceptions. Concurrent changes to the same logger require coordination.
