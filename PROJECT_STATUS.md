# Project Status

## Current status

**Work in progress / research and development project.**

This repository documents and develops the persistent memory subsystem for a personal AI desk robot. It is not a production-ready memory engine and it does not claim perfect semantic understanding.

The project is intentionally being developed in the open so that design decisions, failure modes, experiments and known limitations remain visible over time.

## What already works conceptually

The architecture is built around several principles that are considered stable enough to keep:

- persistent memory should keep source/provenance;
- user corrections must be representable explicitly;
- history should be non-destructive and recoverable;
- stale or conflicting state should be detectable;
- retrieval errors should be distinguishable from storage errors;
- current memory state must never change silently;
- high-impact actions must never be authorized by memory alone.

## What is still experimental

Natural-language interpretation remains the least deterministic part of the system. Current research focuses on practical handling of:

- negation and polarity;
- attribution and quoted speech;
- temporal state changes;
- compound statements;
- conflicting or duplicated memories;
- stale retrieval;
- correction and supersession;
- transaction safety for destructive state transitions.

Several extraction/validation architectures have been explored. Earlier approaches are retained as historical evidence rather than silently replaced.

## Acceptance philosophy

The project does **not** use the standard:

> "The system must never make a semantic memory mistake."

Instead, the operational target is:

> "The system should provide useful everyday memory while making errors traceable, correctable and recoverable, and while preventing those errors from causing irreversible or high-impact harm."

This means some low-risk semantic mistakes can be accepted as known limitations during development.

Examples include:

- remembering that a user drinks coffee when they actually said they do not;
- confusing who said a low-risk fact;
- retrieving an older preference instead of the latest one;
- missing a memory that should have been stored.

These are still bugs. They are simply not treated as automatic blockers when provenance and correction remain intact.

## Hard blockers

The following remain unacceptable:

- unexplained active-state changes;
- silent mutation caused by reads, embeddings, indexing or retrieval;
- irreversible loss of source or history;
- half-applied destructive corrections;
- state changes that cannot be traced to a memory event;
- memory-only authorization of financial, security-sensitive, medical or other high-impact actions.

## Debugging model

When the robot remembers something incorrectly, the intended investigation path is:

1. What was actually said?
2. What did the extraction stage produce?
3. What did validation accept or reject?
4. Which memory event was committed?
5. What became the current state?
6. What did retrieval return later?
7. What context was sent to the response model?
8. What did the robot finally say?

This separation is important because a wrong answer does not necessarily mean the stored memory itself is wrong.

## Development policy

- Use synthetic test data in this public repository.
- Do not commit real conversation logs, private user data, production memory databases, credentials or API keys.
- Record known limitations instead of hiding them.
- Prefer understandable architecture over accumulating one-off semantic patches.
- Preserve historical benchmark results as historical results.
- Use external memory projects and research as references, while keeping licensing and provenance clear.
- Treat this repository as a living engineering record, not a polished final product.

## Near-term direction

The next phase is expected to focus less on proving perfect language semantics and more on:

- reliable event logging and observability;
- explicit current-state derivation;
- easy manual correction;
- practical risk-based memory classes;
- controlled real-world development use;
- comparison with existing open-source memory systems;
- documenting failures observed in actual use.

See also:

- [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md)
- [`docs/WORKFLOW.md`](docs/WORKFLOW.md)
- [`docs/FAILURE_MODES.md`](docs/FAILURE_MODES.md)
- [`SECURITY_AND_PRIVACY.md`](SECURITY_AND_PRIVACY.md)
