# Roadmap

> **Status: planned.** All phases below are future work. This repository is currently in Phase 0 and contains documentation only.

The roadmap prioritizes validation of the narrow coordination loop before expanding scope. No phase implies a commitment to build unnecessary payment, custody, governance, or agent-protocol infrastructure.

## Phase 0 — Concept & Architecture

**Current focus.** Define the product boundary and a credible implementation path.

- Product thesis and coordination-market positioning.
- Competitive research and problem validation.
- Proposed Arkiv market-state model.
- Canonical lifecycle and off-Arkiv execution boundary.
- UI concept and documentation structure.

## Phase 1 — Market State Prototype

**Planned.** Validate whether the proposed market-state loop can be represented and queried as intended.

- Arkiv entities for the narrow core model.
- Typed attributes and clear units.
- Expiration and lifetime behavior.
- Core discovery, quote, and receipt queries.

## Phase 2 — Coordination Engine

**Planned.** Build the application-level flow around validated market state.

- Service requests.
- Temporary offers.
- RFQs and quotes.
- Quote comparison and provider selection.

## Phase 3 — Settlement & Evidence

**Planned.** Connect the coordination loop to settlement and limited completion evidence.

- x402 integration.
- Service receipts.
- Execution evidence and carefully scoped references or hashes.

## Phase 4 — Agent Interface

**Planned.** Make the validated coordination loop accessible to machine participants.

- Buyer-agent integration.
- Provider interface.
- Machine-readable workflows.

## Phase 5 — Reputation & Market Intelligence

**Planned.** Derive transparent insights from the accumulated evidence.

- Performance aggregation.
- Provider evidence views.
- Historical market signals.

## Scope discipline

The roadmap does not currently include tokenomics, governance, a DAO, new smart contracts, SDK implementation, MCP implementation, A2A implementation, a browser extension, a mobile app, AI model training, a complex frontend, or a separate database product. Those are not prerequisites for validating the core market-state coordination problem.
