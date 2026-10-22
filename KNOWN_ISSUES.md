# Known Issues

This project intentionally distinguishes between **known semantic limitations** and **state-integrity/safety defects**.

A known semantic limitation does not automatically block development when the resulting memory remains traceable and correctable. Silent corruption, unexplained mutation and irreversible high-impact failures remain blockers.

## KI-001 — Shared attribution across compound clauses

Example:

> "Jan says that I live in Amsterdam and that I drink coffee."

A model or extraction stage may preserve third-party attribution for one clause but incorrectly turn another clause into a direct personal assertion.

**Impact:** incorrect low-risk personal memory may be stored.

**Current policy:** known semantic limitation. Keep provenance and source evidence; allow explicit user correction. Reassess severity if the same failure affects sensitive/high-impact memory.

## KI-002 — Negation / polarity interpretation

Example:

> "I don't drink coffee."

Natural-language extraction can theoretically misread polarity and store the opposite value.

**Impact:** incorrect conversational memory.

**Current policy:** acceptable only if the write is traceable and correctable. Explicit user corrections should receive higher authority than weak inferred claims.

## KI-003 — Temporal interpretation

Examples include future plans, previous states and transitions such as:

> "Next year I am moving."

> "I used to drink coffee."

A model may confuse past, future and current state.

**Impact:** stale or prematurely active memory.

**Current policy:** known limitation for ordinary conversational facts; continue improving temporal handling and retrieval precedence.

## KI-004 — Retrieval may choose stale/conflicting memory

The correct current memory may exist while retrieval surfaces an older or conflicting claim.

**Impact:** the robot can answer incorrectly even though stored state is correct.

**Current policy:** instrument retrieval so storage errors can be distinguished from retrieval/ranking errors.

## Hard non-acceptable failures

The following are not treated as ordinary known limitations:

- active memory changes without an explicit traceable event;
- silent mutation caused by reads, embeddings, indexing or retrieval;
- irreversible loss of source/provenance without an explicit deletion requirement;
- broken transactional behavior that leaves a destructive correction half-applied;
- memory alone authorizing security-sensitive or other high-impact actions.

## Philosophy

The operational target is not "the system never makes a semantic mistake." The target is:

> Useful everyday memory whose mistakes can be found, explained, corrected and prevented from causing irreversible or high-impact harm.
