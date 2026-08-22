# Conceptual market queries

> **Status: conceptual.** These are product questions and readable pseudo-queries. They are not implemented Arkiv queries, API calls, or supported syntax.

Machina needs to ask structured questions about current supply, request-specific quotes, and historical execution evidence. The eventual query design must be validated against the selected Arkiv integration and the privacy model.

## 1. Find eligible active OCR offers

**Question:** Which active offers can fulfill an OCR request for a buyer's required volume, budget, accuracy, and EU region?

```text
FROM ServiceOffer
WHERE capability = "OCR"
  AND capacity >= requestedVolume
  AND price <= buyerBudget
  AND accuracy >= requiredAccuracy
  AND region = "EU"
  AND status = "ACTIVE"
  AND expiresAt > now
```

This query expresses a coordination need: return only offers that are current and meet the buyer's declared constraints. The buyer can apply its own ranking strategy after receiving eligible results.

## 2. Find active quotes for a request

**Question:** Which unexpired provider quotes are associated with a specific `ServiceRequest`?

```text
FROM Quote
WHERE requestId = "request_123"
  AND status = "ACTIVE"
  AND expiresAt > now
ORDER BY offeredPrice ASC
```

Price ordering is illustrative, not a prescribed selection policy. A buyer might instead prioritize accuracy, latency, provider evidence, or a combination of terms.

## 3. Find a provider's historical receipts

**Question:** What receipt history can be inspected to derive performance evidence for a provider?

```text
FROM ServiceReceipt
WHERE provider = "provider_abc"
  AND executionStatus IN ("COMPLETED", "FAILED", "DISPUTED")
  AND executedAt >= lookbackWindow
ORDER BY executedAt DESC
```

A future client or analytics layer could use the result set to derive, subject to a defined methodology:

- completion rate;
- average latency;
- dispute rate;
- completed jobs; and
- recent activity.

A receipt history is evidence to evaluate, not an automatic or immutable reputation score. Definitions, trust assumptions, and dispute semantics remain future design work.

## Query design notes

- Numeric fields need explicit units and deterministic representations before implementation (for example, scaled integer amounts and latency in milliseconds).
- Active market queries must respect the proposed expiry semantics.
- A queryable market state should expose only non-sensitive attributes. Private payloads and raw outputs remain off Arkiv.
- Query syntax, indexes, pagination, authorization, and aggregation are intentionally out of scope until an Arkiv prototype is validated.
