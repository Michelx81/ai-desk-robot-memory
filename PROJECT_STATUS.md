# Project Status

## Current status

**Work in progress / research and development project.**

This repository documents and develops the persistent memory subsystem for a personal AI desk robot. It is not a production-ready memory engine and it does not claim perfect semantic understanding.

The project is intentionally being developed in the open so that design decisions, failure modes, experiments and known limitations remain visible over time.

### Status update — 29 October 2026

The current development focus is no longer only model quality. A substantial part of the work is now devoted to **evaluation integrity, provenance and benchmark governance**.

Candidate G currently uses a deliberately narrow routing contract with the following outcomes:

- `NONE`
- `lives_in`
- `planned_lives_in`
- `works_at`
- `planned_works_at`
- `favorite_color`

Compound or unsupported requests are expected to fail closed to `NONE` rather than activating multiple or uncertain memory routes.

Historical Candidate G development evidence reached 170/170 on its then-authorized development suites. That result is retained as historical development evidence only; it is **not** treated as a final holdout result or production-readiness claim.

A prospective acceptance policy has been frozen before any new holdout authoring or inference. The planned final evaluation contains 152 cases: 75 positive cases and 77 `NONE` cases, with balanced per-route and negative-family quotas. Passing requires at least 147/152 semantically correct cases together with stricter route-level and false-activation constraints.

The final holdout has **not** been authored, frozen or executed yet.

Before that can happen, the project is completing a development-reference and contamination audit. During that audit, one candidate development source was rejected for benchmark-reference purposes after provenance review, and the reference set was rebuilt from a clean baseline. This is treated as an evaluation-governance incident rather than silently absorbed into the benchmark.

Current governance rules include:

- protected historical holdout material remains closed and is not used to build the new development reference;
- development text used for contamination screening must have explicit provenance;
- benchmark authoring, adjudication and checking roles remain separated;
- tools or sessions that inspect development case text are excluded from future blind holdout-author/adjudicator roles;
- no Candidate G holdout inference is permitted until the development-reference audit is complete;
- no production integration or next-stage memory architecture work is authorized from an incomplete benchmark.

The public GitHub repository contains only public-safe documentation and synthetic examples. Restricted development packets, personal conversation data, protected benchmark material, local databases and credentials remain outside the repository.

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
- transaction safety for destructive state transitions;
- benchmark contamination detection and evaluation provenance.

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
- memory-only authorization of financial, security-sensitive, medical or other high-impact actions;
- declaring benchmark success when the evaluation reference or contamination status is unresolved.

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
- Keep development, holdout, authoring and adjudication roles distinct where benchmark integrity depends on them.
- Use external memory projects and research as references, while keeping licensing and provenance clear.
- Treat this repository as a living engineering record, not a polished final product.

## Near-term direction

The immediate next step is to finish the development-reference/provenance audit before any new holdout authoring or Candidate G holdout inference.

After benchmark governance is clean enough to proceed, the project can return to the broader memory roadmap:

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
