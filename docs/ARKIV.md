# Arkiv's proposed role

> **Status: conceptual.** This document explains why Machina may benefit from Arkiv. It does not claim an existing Arkiv integration or guarantee a specific capability until validated against Arkiv's documentation and a prototype.

Arkiv is proposed as Machina's shared market-state layer: a place for structured, attributable, time-scoped facts that independent market participants can query when coordinating work.

## Why Arkiv fits the proposed model

Machina needs more than static discovery. It needs to model offers, requests, quotes, availability, and limited execution evidence whose relevance changes over time. The proposed model uses Arkiv for four product consequences:

1. **Queryable entities.** Agents can locate structured market records by machine-readable attributes such as capability, price, capacity, region, status, and performance requirements.
2. **Time-scoped market state.** Requests, availability signals, offers, and quotes can be designed with explicit lifetimes, so market participants have a clear validity window for current terms.
3. **Verifiable provenance.** Creator and ownership context can help participants assess who published a market record instead of treating an operator's database attribution as the only source of identity.
4. **Historical evidence.** Longer-lived receipts can contribute evidence about completed interactions, rather than relying exclusively on editable marketplace ratings.

The model should validate the exact Arkiv primitives, lifecycle semantics, and query behavior before any implementation claim is made.

## Proposed entity use

| Entity | Proposed role | Time horizon |
| --- | --- | --- |
| `ServiceOffer` | Current provider capability and terms | Short-lived |
| `ServiceRequest` | Buyer demand and constraints | Short-lived to deadline |
| `Quote` | Provider terms for a specific request | Short-lived |
| `ServiceReceipt` | Limited completion and performance evidence | Longer-lived |

Potential provider, availability, and reputation concepts should be added only when the core loop demonstrates a need for them.

## Expiration is product behavior

An offer with no remaining capacity or an expired quote should not continue to be treated as current supply. In a conventional application, this commonly relies on application code, cleanup jobs, and an operator's interpretation of status.

Machina's proposed Arkiv model instead treats expiry as an explicit property of the market record and query semantics to validate. This is not merely a storage optimization: it makes validity part of the coordination contract between independent participants.

## Provenance and historical evidence

For a market record, the important question is not only what it says but who published it and when. A future Arkiv-backed model should preserve creator/owner context for offers, requests, quotes, and receipts. That lets a participant distinguish a provider-published quote from an operator's assertion about a provider.

Likewise, receipts can provide inspectable historical facts—such as a declared execution time, status, payment reference, and appropriate hash—without claiming that a single receipt proves service quality or resolves disputes. Reputation should be a transparent, derived interpretation of evidence, not an opaque score.

## What stays off Arkiv

Not everything belongs on Arkiv. The proposed boundary excludes:

- private prompts, customer data, and sensitive execution payloads;
- secrets, private keys, and API credentials;
- large files and raw service outputs;
- private computation and the service's actual execution path;
- x402 payment execution; and
- latency-critical matching or execution logic where public market state is not the right tool.

These remain with the buyer, provider, or appropriate off-Arkiv systems. A `ServiceReceipt` can reference a result hash where appropriate without publishing the underlying result.

## Counterfactual: operator-controlled Postgres

Postgres can absolutely filter and sort a marketplace. The difference is not that a conventional database cannot answer the queries. The difference is where the coordination guarantees and trust assumptions live.

| Concern | Operator-controlled database | Proposed Arkiv market state |
| --- | --- | --- |
| Temporary offers | The operator's application, jobs, and status conventions decide when stale state disappears. | Explicit time-scoped records can make validity a shared, inspectable market property. |
| Attribution | Participants trust the operator's record of who authored an offer or receipt. | Creator/owner context can be evaluated as part of the record's provenance. |
| Historical evidence | Records and ratings are controlled by the marketplace operator's retention and editing policy. | Receipts can provide attributable historical evidence for independent inspection. |
| Market queries | Filtering depends on the operator exposing and faithfully serving the desired view. | Structured shared entities can support machine-readable market questions. |

This does not eliminate all trust, solve service quality, or make an operator unnecessary. It makes specific coordination facts less dependent on a single operator's private database. The architecture is therefore deliberate: use Arkiv where shared, attributable, time-scoped state matters, and keep private or execution-sensitive data elsewhere.
