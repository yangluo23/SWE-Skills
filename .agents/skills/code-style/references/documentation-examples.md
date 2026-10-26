# Documentation that explains a contract

These independent examples demonstrate depth and placement of explanation.
They are not implementations to paste into a project. Describe the behavior
that exists in the target code, and retain its documentation dialect.

## Select examples by caller risk

| Caller question | Useful example | Owning location |
|---|---|---|
| When does a staged change become visible? | Construct, stage, inspect, publish, inspect again. | Class or lifecycle guide. |
| Are some counts included in other counts? | A small input and the normalized total. | Normalization function. |
| Does missing information mean zero? | An unknown input and the explicit unknown result. | Function defining the distinction. |
| May the caller mutate this result? | Show independent data or a deliberately borrowed view. | Returning interface. |
| What happens after execution fails? | Show the persisted status or exception boundary. | Runtime/runner guide. |

Simple accessors, package markers and private forwarding helpers usually need
no example. A class-level example can cover several related methods; its
presence does not remove the need for a distinct example of a surprising
method-specific rule.

An executable example needs its own imports and legal setup. It should expose
the outcome being taught, rather than print an entire nested structure.
Keep filesystem examples in temporary directories and use synthetic data.
For paid or service-dependent workflows, provide a clearly labeled fragment
and verify it against actual call sites and existing integration tests.
Do not present a fake runtime as if it were the production integration.

## A public stateful interface

This self-contained example uses prose and Google-style sections. The
docstrings explain what callers may do; the design note explains why the
implementation prevents a particular mutation.

```python
class MessageBuffer:
    """Collect ordered messages until a batch is sealed for delivery.

    Each instance owns one in-memory batch. Create a new buffer for a new
    delivery. Sealing is irreversible: existing messages remain readable,
    but the buffer accepts no further appends. Instances are not thread-safe.

    Examples:
        >>> buffer = MessageBuffer()
        >>> buffer.append("Request accepted.")
        >>> buffer.seal()
        ('Request accepted.',)
        >>> buffer.seal()
        ('Request accepted.',)
    """

    def __init__(self) -> None:
        self._messages: list[str] = []
        self._sealed = False

    # Note [Sealed message batches]
    # A delivery receipt describes one fixed, ordered batch. Allowing later
    # appends would make a repeated read disagree with that receipt. Freeze
    # further writes after sealing and return immutable batch values; this
    # does not provide persistence or synchronization between threads.

    def append(self, message: str) -> None:
        """Append a message verbatim to the unsealed batch.

        Empty strings are retained and ordering follows append order.

        Raises:
            RuntimeError: If the batch has already been sealed. The batch
                remains unchanged on this failure.
        """
        if self._sealed:
            raise RuntimeError("Cannot append to a sealed message batch")
        self._messages.append(message)

    def seal(self) -> tuple[str, ...]:
        """Prevent further appends and return the batch in arrival order.

        Repeated calls return equal values without adding messages or
        delivering the batch. The caller performs delivery separately.

        Returns:
            An immutable tuple of strings, independent of future buffer
            instances. An empty buffer returns an empty tuple.
        """
        # See Note [Sealed message batches] for the write restriction.
        self._sealed = True
        return tuple(self._messages)
```

This example states the legal sequence, the meaning of an empty input,
failure effects, repeat-call behavior, result ownership and concurrency
limits. It does not claim persistence, crash recovery or actual delivery.
The repeated read in the example demonstrates a useful contract rather
than merely displaying the constructor's signature.

## Choose comments that protect reasoning

Avoid comments such as `# Update the seed` or `# Call the model`. They
repeat the statements and do not help a maintainer change them safely.

A useful explanation identifies the mechanism and consequence, for example:

```python
# Replays borrow the caller's runtime. Restore its previous seed even if
# execution fails, so a comparison does not change later official tasks.
previous_seed = runtime.seed
runtime.seed = replay_seed
try:
    result = execute_replay()
finally:
    runtime.seed = previous_seed
```

This is an illustrative fragment with a caller-provided runtime and replay
function. It does not provide synchronization for concurrent use of that
runtime. Document such a limitation when shared use is possible; do not
infer thread safety from the presence of `try/finally`.

## Review existing documentation

Read the body and callers before expanding a docstring. Useful questions:

- Does `None` disable behavior, mean unknown, or select a default?
- Is a returned mapping a fresh copy, a borrowed view or mutable shared state?
- Is this operation only staging work, or does it publish visible state?
- If it fails, what has already changed and what must the caller clean up?
- Is a retry safe, and what identity or journal makes that true?
- Does an example use real names and show a legal sequence?

Apply only the questions relevant to the interface. A trivial private
wrapper does not need a miniature manual; a public lifecycle method often
needs more than a one-line summary.
