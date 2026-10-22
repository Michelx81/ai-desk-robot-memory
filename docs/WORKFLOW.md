# Memory Workflow

This document describes the intended operational workflow of the AI Desk Robot memory subsystem and the development method used while the system is still a work in progress.

## 1. Runtime memory path

The desired path is:

```text
Conversation
    ↓
Source / conversation event
    ↓
Memory extraction
    ↓
Validation / policy
    ↓
Memory event or claim
    ↓
Current memory view
    ↓
Retrieval
    ↓
Context supplied to the response model
    ↓
Robot response
```

Each step should remain distinguishable in logs or stored provenance. A wrong final answer should not be reduced to only "memory was wrong"; the system should allow us to identify which stage failed.

## 2. Source first

The original conversational source is the evidence for why a memory exists.

For development and debugging, a stored memory should be traceable back to at least:

- the source event or turn;
- the relevant source text or evidence span where practical;
- the extraction or proposal that created the claim;
- the mutation event that changed active state.

The repository itself must use synthetic examples. Real personal conversation logs and production memory databases must not be committed.

## 3. Extraction

A language model may identify candidate memories from normal conversation.

Example:

```text
Michel: "No, that is not right. I don't like coffee."
```

Possible extracted meaning:

```text
subject = user
predicate = likes_coffee
value = false
```

Extraction is probabilistic and may be wrong. Semantic perfection is not a hard requirement for ordinary low-risk conversational facts.

## 4. Validation and policy

Before a candidate changes memory state, deterministic checks should enforce structural and state-integrity rules where possible.

Examples:

- a mutation must have a traceable cause;
- stale writes should not silently replace newer state;
- destructive replacements should be atomic or recoverable;
- history/provenance should not disappear as a side effect;
- memory must not independently authorize high-impact actions.

Natural-language meaning may still remain partially model-dependent.

## 5. Memory events instead of silent edits

An active memory should not simply flip from one value to another without a recorded event.

Preferred correction model:

```text
old claim: likes_coffee = true
status: superseded

correction event:
source: explicit user correction
old value: true
new value: false

new active claim: likes_coffee = false
```

The exact internal representation can evolve. The invariant is more important than the representation:

> No unexplained memory mutation.

Reading, retrieval, embeddings, indexing and maintenance tasks must not silently change canonical/current memory state.

## 6. Current memory view

The current view is the robot's best-known active interpretation of memory, not an assertion of perfect objective truth.

Old, disputed or superseded claims may remain in history while only the currently selected claim is normally supplied to retrieval.

Explicit user corrections should normally outrank weak inferred or third-party claims.

## 7. Retrieval

Stored state and retrieved context are separate concerns.

A correct database can still produce a wrong answer if retrieval selects an old or conflicting claim. Debugging therefore needs to preserve the distinction between:

```text
stored current state
        vs
retrieved context
        vs
final response
```

## 8. User correction

User correction is a normal part of the operating model, not an exceptional failure mode.

Example:

```text
Robot: "You like coffee."
User:  "No. I don't like coffee."
```

Expected behavior:

1. record the new source statement;
2. interpret it as an explicit correction where possible;
3. preserve the previous claim in history;
4. make the corrected claim the active/current view;
5. retrieve the corrected view in future conversations.

Manual inspection or manual correction is acceptable during early development. A complete automated administration UI is not required for the first usable versions.

## 9. Development workflow

This project is intentionally iterative.

For each meaningful architecture change:

1. preserve the previous working state in version control;
2. define what problem the change is intended to solve;
3. use synthetic regression cases and adversarial examples;
4. distinguish semantic quality errors from state-integrity failures;
5. record newly discovered limitations in `KNOWN_ISSUES.md`;
6. keep historical benchmark results historical — do not silently rewrite prior results;
7. prefer a small understandable change over an expanding chain of special-case patches;
8. collect real development evidence before declaring a known limitation a release blocker.

## 10. Practical acceptance policy

Not every semantic mistake blocks development.

A low-risk memory error may be accepted when it is:

- traceable;
- visible during debugging;
- correctable;
- non-destructive;
- unable to authorize a high-impact action by itself.

State corruption, unexplained mutation, irreversible provenance loss and broken destructive transactions remain hard failures.

## 11. Work in progress

The architecture is expected to change. This document describes the intended behavioral contract, not a frozen implementation specification.
