# Market lifecycle

> **Status: conceptual.** This is the canonical proposed Machina lifecycle. None of its steps are implemented by this repository.

Machina is designed around time-sensitive service coordination. The lifecycle begins with a buyer need and ends with limited evidence that can inform future decisions.

```text
publish → discover → request quote → compare → select → settle → execute → receipt → reputation evidence
```

## 1. Publish

Providers publish temporary `ServiceOffer` records that describe current service terms, such as capability, price, capacity, region, availability, and expiration. A buyer can also publish a `ServiceRequest` to express specific demand.

## 2. Discover

A buyer filters active offers against its constraints. A provider may similarly discover an active request it can serve. Discovery must account for expiry: a record that is no longer valid should not be treated as active market state.

## 3. Request quote

When a general offer is insufficient, the buyer requests a quote or RFQ response. A provider returns a time-bounded `Quote` for the request, including price, capacity, and expected performance.

## 4. Compare

The buyer compares eligible offers or quotes against its own strategy: required quality, price ceiling, capacity, region, deadline, latency, and prior evidence. Machina does not prescribe a universal ranking algorithm.

## 5. Select

The buyer chooses a provider and agreed terms. Selection is a coordination decision; it is not the same as custody, payment processing, or execution.

## 6. Settle

The buyer pays the selected provider through x402. x402 is the proposed settlement rail, while the market-state model remains distinct from payment execution.

## 7. Execute

The provider executes the actual service off Arkiv. Private prompts, credentials, customer data, raw inputs, raw outputs, and sensitive execution payloads do not belong in the proposed public market-state layer.

## 8. Receipt

A `ServiceReceipt` records limited completion and performance evidence, with a payment reference and result hash where appropriate. The receipt should not expose raw service output or sensitive data.

## 9. Reputation evidence

Over time, receipts may support derived evidence such as completed jobs, completion rate, recent activity, latency, or disputes where a suitable data model exists. This is a future analytical use, not a claim of an implemented reputation system.

## Illustrative OCR scenario

A buyer agent needs OCR for 10,000 documents in the EU, requires at least 99% accuracy, and has a budget no greater than $8.

1. Providers publish or return time-bounded terms.
2. The buyer discovers offers and requests a quote where necessary.
3. It removes offers that do not meet the 10,000-document, EU, 99% accuracy, and $8 constraints.
4. It compares remaining eligible providers and selects one.
5. The buyer settles through x402; the selected provider executes OCR outside Arkiv.
6. A receipt can capture non-sensitive completion evidence for a later evaluation.

The concrete provider figures and selection are documented in the [OCR example](../examples/ocr-market/README.md). They are illustrative and do not represent live availability.
