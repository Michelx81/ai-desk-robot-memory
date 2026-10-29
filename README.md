# AI Desk Robot Memory

Persistent, auditable memory architecture for a personal AI desk robot, with provenance, corrections, conflict handling and recoverable state.

## Status

**Work in progress / research prototype.**

This repository tracks the memory subsystem for a personal AI desk robot. The goal is not perfect semantic memory. The goal is a system that is useful in everyday conversation while keeping memory changes traceable, correctable and recoverable.

As of **29 October 2026**, development is intentionally paused at the evaluation-governance boundary while the Candidate G development-reference and contamination audit is completed. Historical development results are preserved, but no new final holdout has been authored, frozen or executed and no Candidate G holdout inference is authorized yet.

For the current development status, acceptance philosophy, benchmark-governance state, hard blockers and near-term direction, see [`PROJECT_STATUS.md`](PROJECT_STATUS.md).

Current development priorities:

- persistent conversational memory;
- provenance: where a memory came from;
- explicit corrections and supersession;
- conflict and stale-state handling;
- no unexplained memory mutation;
- reversible / non-destructive history;
- observability and debugging of the path from conversation to stored memory to retrieval;
- practical, risk-based acceptance of known semantic limitations;
- reproducible benchmark provenance and contamination control.

## Core principle

> Memory may be imperfect. Memory state must not change without an explainable event.

Semantic mistakes in low-risk conversational memory can be tolerated when they are visible, traceable and correctable. Silent state corruption, unexplained mutation, irreversible history loss and memory-only authorization of high-impact actions are not acceptable.

The same traceability principle now also applies to evaluation: benchmark success must not be claimed while development-reference provenance or contamination status is unresolved.

## How it works

The intended memory path is documented in [`docs/WORKFLOW.md`](docs/WORKFLOW.md):

```text
Conversation → source event → extraction → validation → memory event → current view → retrieval → response
```

Known failure locations and a practical diagnostic sequence are documented in [`docs/FAILURE_MODES.md`](docs/FAILURE_MODES.md).

Project-wide known semantic limitations and hard non-acceptable failures are tracked in [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md).

## Repository safety

This repository must not contain real personal conversation logs, production memory databases, API keys, credentials or other private user data. Test data should be synthetic.

Protected benchmark material and restricted local development-reference packets are also kept out of the public repository.

See [`SECURITY_AND_PRIVACY.md`](SECURITY_AND_PRIVACY.md) and `.gitignore` before importing local development files.

## Project scope

This repository is for the **memory subsystem** only. Physical robot hardware, display, servo, audio and other device code should remain separate unless a memory-specific integration requires otherwise.

## Current architecture history

The project has explored multiple memory extraction and validation approaches, including proposal-based extraction, structured batches, staged semantic compilation and a newer transaction-oriented prototype. Earlier experimental results remain historical evidence; newer development work must not silently overwrite those results.

The current direction is pragmatic: preserve strong state-integrity guarantees while relaxing the requirement that natural-language extraction itself be semantically perfect. Candidate G adds a deliberately narrow routing layer, while the current audit work focuses on proving that future evaluation can be run against a clean, prospectively governed benchmark.

## License

No public license has been selected yet. The repository is public for transparency and collaborative learning, but no reuse license has been granted at this stage.
