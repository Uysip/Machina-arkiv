# OCR market example

> **Illustrative scenario only.** This example describes a possible future Machina interaction. The providers, prices, capacity, latency, queries, settlement, and receipt are not live or implemented.

## Buyer requirement

A buyer agent expresses this constraint set:

> “I need OCR for 10,000 documents, EU region, >=99% accuracy, budget <= $8.”

In a future market-state model, the request would also carry an expiry and deadline so it is not treated as ongoing demand indefinitely.

## Temporary provider offers

| Provider | Price | Accuracy | Latency | Capacity | Eligibility for this request |
| --- | ---: | ---: | ---: | ---: | --- |
| Provider A | $7.50 | 99.3% | 180 ms | 15k documents | Meets stated constraints |
| Provider B | $6.00 | 98.7% | 90 ms | 20k documents | Does not meet 99% accuracy |
| Provider C | $7.00 | 99.8% | 220 ms | 10k documents | Meets stated constraints |

The buyer can query and compare offers based on its constraints. Provider B is cheaper and faster but falls below the required accuracy. Providers A and C both meet the stated requirements.

## Selection and follow-through

Provider C may be selected because it satisfies the 99% accuracy requirement while remaining within the budget and capacity constraints. The buyer's actual ranking policy could make a different choice if it weighs latency, price, or historical evidence differently.

The proposed next steps are deliberately separated:

```text
x402 → payment
service → execution outside Arkiv
receipt → limited evidence for later evaluation
```

A future receipt might record a payment reference, execution status, timestamp, latency or performance evidence, and a result hash where appropriate. It would not publish the raw documents, OCR output, credentials, or private execution payloads.
