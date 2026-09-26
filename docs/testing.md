---
title: Testing
layout: default
nav_order: 20
---

# Testing

[Documentation home](index.md)

## Build and lint

From the repository root:

```bash
npm ci
npm run lint
npm run build
```

These are the checks exposed by `package.json`. The repository currently has no automated test suite or `npm test` script.

## Manual chat checks

Run `npm run dev` and open `http://localhost:3000`:

- Request a coffee and inspect the merchant quote, policy decision, selected chain, and simulated receipt.
- Request a purchase that exceeds the auto-approval threshold and verify that the approval prompt appears before execution.
- Reject a pending request and confirm that no payment receipt is generated for it.
- Restart the server and confirm that state resets, as documented in [Data Store](data-store.md).

The parser accepts the intents documented in [Intent Parser](intent-parser.md). Receipts and transaction hashes come from the mock executor; they are not evidence of a blockchain transaction. See [Policy Engine](policy-engine.md) for thresholds and decision paths.
