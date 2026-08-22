# Proposed data model

> **Status: conceptual.** The entities and fields below are a proposed data model for future validation. They are not an implemented schema, API, or Arkiv integration.

The initial model keeps the market legible without overloading it with every possible supporting object. `Provider`, `Availability`, and `ReputationEvidence` may become supporting concepts later; they are not required as standalone entities in this foundation.

## Shared design principles

- **Time-scoped active state:** market records that describe current opportunities should include an expiry or lifetime.
- **Provenance:** a future Arkiv-backed record should retain a creator/owner context so participants can distinguish a participant-published record from an operator assertion.
- **Minimal public facts:** only structured market facts and limited execution evidence belong in this proposed layer. Sensitive content remains off Arkiv.
- **Relationships by identifier:** future records can link requests, offers, quotes, and receipts through stable identifiers; the identifier format is intentionally undecided.

## `ServiceOffer`

**Purpose.** A provider's current, temporary statement of capability and terms. It may be published for general discovery or associated with a particular request.

**Key fields.** Provider identity; capability; price and currency/unit; capacity; region; availability; service constraints; declared performance or SLA; payment rail; status; expiration; optional `requestId`.

**Relationships.** May respond to a `ServiceRequest`, inform a `Quote`, and be referenced by a subsequent `ServiceReceipt`.

**Lifetime.** Short-lived by design. Once expired, it should not be considered eligible active supply; the exact enforcement and retention semantics remain to be validated.

**Creator / owner / provenance.** The provider should be attributable as the original publisher. Where ownership matters, a future model should preserve the relevant ownership context without treating it as proof of service quality.

## `ServiceRequest`

**Purpose.** A buyer's time-scoped expression of service demand.

**Key fields.** Buyer identity; capability; required volume; maximum price; required accuracy or SLA; region; deadline; service constraints; status; expiration.

**Relationships.** Can receive one or more `Quote` records and may be matched to relevant `ServiceOffer` records. A completed request can be referenced by a `ServiceReceipt`.

**Lifetime.** Active only through its deadline or explicit expiry. An expired request should not solicit new quotes or be treated as open demand.

**Creator / owner / provenance.** The buyer should be attributable as creator. The future model should make clear whether any later state transition is authored by the buyer, provider, or an external coordination process.

## `Quote`

**Purpose.** A provider's response to a specific request or RFQ, with terms that can be compared by the buyer.

**Key fields.** Request ID; provider identity; offered price; capacity; SLA or expected performance; payment rail; status; expiration; optional references to the source offer.

**Relationships.** Belongs to one `ServiceRequest`, may be derived from a `ServiceOffer`, and can provide the selected terms later reflected in a receipt.

**Lifetime.** Deliberately short-lived. Expiry would prevent a buyer from treating stale pricing or capacity as current.

**Creator / owner / provenance.** The provider should be attributable as quote creator. Future selection semantics should preserve which terms were selected without implying automatic settlement.

## `ServiceReceipt`

**Purpose.** Limited, longer-lived evidence that a selected service interaction reached an execution state.

**Key fields.** Request ID; provider; buyer; service or capability; amount; execution status; execution timestamp; latency or performance evidence; result hash where appropriate; payment reference; optional quote or offer reference.

**Relationships.** References the completed `ServiceRequest` and may link to the accepted `Quote` or `ServiceOffer`. A future aggregation layer could derive reputation evidence from multiple receipts.

**Lifetime.** Expected to outlive active offers, requests, and quotes so that historical evidence can be inspected. Retention policy and any redaction approach remain open design questions.

**Creator / owner / provenance.** The future design must identify who published a receipt and what each field attests to. A receipt is evidence, not a blanket guarantee that every participant accepts its contents.

## Supporting concepts, intentionally deferred

- **Provider:** an identity and possibly a public provider profile, rather than a necessary source of truth for every service claim.
- **Availability:** a short-lived signal that may be represented within offers before becoming its own entity.
- **ReputationEvidence:** a derived view over receipts and other verified facts, not a simplistic editable rating.
