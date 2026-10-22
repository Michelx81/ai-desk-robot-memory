# Failure Modes

This document maps common failure classes to the stage where they occur. The purpose is diagnosis: when the robot gives a wrong answer, determine **where** the error entered the pipeline.

## FM-001 — Extraction misses a fact

Source:

```text
"I don't drink coffee."
```

Failure:

No memory candidate is created.

Result:

The robot may later behave as if it never learned the preference.

Classification:

Ordinary semantic quality failure for low-risk memory.

## FM-002 — Polarity / negation is inverted

Source:

```text
"I don't drink coffee."
```

Wrong extraction:

```text
drinks_coffee = true
```

Result:

An incorrect memory may be stored.

Classification:

Known semantic limitation when traceable and correctable. More serious if applied to sensitive or high-impact state.

## FM-003 — Attribution is lost

Source:

```text
"Jan says that I live in Amsterdam and that I drink coffee."
```

Failure:

A third-party statement is converted into a direct assertion by the user.

Result:

The robot may store a personal fact that the user never asserted as true.

Classification:

Known semantic limitation; provenance should make the error diagnosable.

## FM-004 — Temporal state is misread

Source:

```text
"I used to drink coffee."
```

Wrong interpretation:

```text
current drinks_coffee = true
```

Or:

```text
"Next year I am moving to Utrecht."
```

becomes an active current residence too early.

Classification:

Known semantic limitation for ordinary conversational memory.

## FM-005 — Correction is not recognized

Existing memory:

```text
likes_coffee = true
```

User says:

```text
"No, that is wrong. I don't like coffee."
```

Failure:

The new statement is ignored, treated as unrelated, or stored without replacing/superseding the old active view.

Result:

The robot may repeat the same mistake later.

Classification:

Important quality failure. Explicit corrections should receive special attention because they are the main recovery mechanism for imperfect semantic memory.

## FM-006 — Partial destructive correction

Intended transition:

```text
old = true
new = false
```

Failure:

The old active claim is retracted but the replacement claim is not committed, or vice versa.

Result:

The memory state becomes inconsistent because one half of a logically linked transition succeeded.

Classification:

Hard state-integrity failure. Destructive/replace-style transitions should be atomic or recoverable.

## FM-007 — Silent mutation

Stored state changes from:

```text
drinks_coffee = false
```

to:

```text
drinks_coffee = true
```

without a new source or explicit mutation event.

Possible causes include software bugs, migrations, accidental writes during indexing, or state reconstruction errors.

Classification:

Hard failure. This is not accepted as an ordinary model-memory limitation.

Invariant:

> Every active-state change must have an explainable event.

## FM-008 — Stored state is correct, retrieval is wrong

Database/current view:

```text
drinks_coffee = false
```

Historical claim:

```text
drinks_coffee = true  [superseded]
```

Failure:

Retrieval supplies the historical claim instead of the active one.

Result:

The robot answers incorrectly even though storage is correct.

Classification:

Retrieval/ranking bug, not a storage bug.

## FM-009 — Retrieval omits relevant memory

The correct active memory exists but is not retrieved when relevant.

Result:

The robot may answer generically, contradict the user, or ask for information it already knows.

Classification:

Ordinary retrieval quality problem unless high-impact behavior depends on it.

## FM-010 — Correct retrieval, wrong final answer

Stored state and retrieved context are correct, but the response model misuses or contradicts them.

Classification:

Response-generation failure, not a memory mutation failure.

## FM-011 — Conflicting active claims

Example:

```text
favorite_color = blue   [active]
favorite_color = green  [active]
```

when the predicate is intended to have a single current value.

Result:

Retrieval may become unstable or arbitrary.

Classification:

State/policy issue. Conflicts may be preserved historically, but the current-view policy should make the ambiguity explicit rather than silently selecting an arbitrary value.

## FM-012 — Stale write wins

A delayed or retried operation based on older state overwrites a newer correction.

Example:

1. old state: coffee = true;
2. user correction commits coffee = false;
3. delayed processing from step 1 later writes coffee = true again.

Classification:

Hard state-integrity failure. Stale-state/version protection should prevent this.

## FM-013 — Manual correction is overwritten by weaker evidence

A deliberate user/manual correction exists:

```text
coffee = false
source = explicit user correction
```

Later, a weak or ambiguous statement is treated as stronger and replaces it.

Classification:

Authority/precedence failure. Manual and explicit corrections should normally outrank weak inferred evidence.

## FM-014 — Provenance is lost

A memory exists but it is no longer possible to determine where it came from or why its state changed.

Classification:

Hard observability/recoverability failure when provenance was expected to be retained.

## FM-015 — Memory is used as authorization

Example:

A stored claim says that the user previously approved a payment, security action or other high-impact operation, and that memory alone is treated as current authorization.

Classification:

Hard safety failure. Conversational memory must not by itself authorize high-impact actions.

# Diagnostic method

For a wrong answer, inspect in this order:

```text
1. What did the user actually say?
2. What did extraction produce?
3. What did validation accept/reject?
4. Which mutation event was recorded?
5. What is the current active state?
6. What did retrieval return?
7. What context reached the response model?
8. What did the response model say?
```

The first stage where expected and observed behavior diverge is the primary failure location.

# Severity principle

A semantic error about an ordinary preference can be an accepted known limitation if it is traceable and correctable.

An unexplained state mutation, half-applied destructive update, irreversible provenance loss, stale write that reverses a correction, or memory-only high-impact authorization remains a blocker.
