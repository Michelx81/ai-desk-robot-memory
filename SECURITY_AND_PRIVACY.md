# Security and Privacy

This repository contains architecture and code for a personal AI memory system. Development must assume that real runtime data can contain sensitive personal information.

## Do not commit

Never commit any of the following:

- real conversation logs or transcripts;
- production SQLite memory databases or snapshots;
- API keys, access tokens, passwords or credentials;
- `.env` files containing secrets;
- real health, financial, legal or identity data;
- unredacted exports from the running robot;
- local backups containing user memory;
- provider request/response dumps containing personal data.

## Test data

Tests and examples should use synthetic data and fictitious people unless a specific public-domain/public-source fixture is intentionally required.

## Memory integrity rule

A change to active memory state must be attributable to an explicit event or operation. Reads, retrieval, indexing, embeddings, ranking and maintenance processes must not silently mutate canonical memory state.

## Corrections and history

User corrections should be represented as explicit corrections/supersession rather than destructive history removal wherever practical. Historical provenance should remain inspectable unless explicit deletion/forgetting requirements apply.

## High-impact actions

Conversational memory alone must never authorize high-impact actions such as payments, credential changes, security-sensitive operations, or medical/legal decisions. Such actions require a separate authorization/confirmation path.

## Before importing local code

Before adding files from a local development directory:

1. inspect the directory for secrets and personal data;
2. verify `.gitignore` coverage;
3. exclude runtime databases, logs, exports and review bundles;
4. inspect staged files before committing;
5. use synthetic fixtures for reproducible tests.
