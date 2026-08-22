# Machina

> **Machine-native coordination marketplace for autonomous agents.**

**Status: Concept / Architecture.** Machina is an early-stage product and architecture concept. Implementation is planned but has not shipped; this repository contains no live marketplace, Arkiv integration, x402 integration, or agent service.

<!-- Future visual: assets/machina-cover.png -->

## Thesis

Autonomous agents increasingly procure machine-accessible services—OCR, inference, translation, data enrichment, compute, verification, and specialized APIs. Discovery and payment are necessary, but they do not by themselves coordinate a temporary market. Prices, capacity, latency, and availability can change faster than a persistent directory can describe them.

Machina is the coordination layer between discovery and settlement. It is designed to help autonomous participants express demand, compare temporary supply, request and evaluate quotes, select providers, and retain execution evidence.

## The problem

A buyer agent needs to source a service against concrete constraints: price, capacity, latency, region, quality, availability, and deadline. Static listings and provider claims do not reliably answer what is available now, whether a quote is still valid, or what evidence supports a provider's history.

Machina proposes shared, machine-readable market state for this coordination problem—not a new payment network or generic service directory.

## What Machina is

- A proposed coordination marketplace for machine-to-machine services.
- A model for short-lived offers, service requests, RFQs, quotes, selection, and execution evidence.
- An architecture in which Arkiv is the queryable, time-scoped market-state layer.
- A complementary consumer of x402 as a payment and settlement rail when a buyer pays a provider.

## What Machina is not

- A generic API directory or traditional API marketplace clone.
- A payment processor, wallet, custody product, or replacement for x402.
- An AI model or a replacement for MCP or A2A.
- A commitment to put private execution data, credentials, or raw outputs on Arkiv.

## How it works

In the proposed flow, a buyer expresses a service need, providers expose current offers or respond with quotes, and Machina coordinates comparison and selection. Payment and service execution occur through the relevant external systems. A resulting receipt can preserve limited, non-sensitive evidence of the completed interaction.

```text
Buyer Agent → Service Request → Machina Market → Temporary Provider Offers
    → RFQ / Quote → Comparison → Provider Selection → x402 Settlement
    → Service Execution → Service Receipt → Reputation / Market Evidence
```

This is the canonical lifecycle described by this repository; it is not a claim that the flow is implemented today. See [the market lifecycle](docs/MARKET_LIFECYCLE.md).

## Why Arkiv

Machina proposes Arkiv for public, structured market state where its time-scoping, queryability, and provenance model can be useful. The concrete product consequences are:

- **Temporary state:** offers and requests can carry explicit expiry semantics, rather than active status depending only on marketplace cleanup jobs.
- **Provenance:** participants can inspect who created a market record rather than relying solely on an operator-maintained attribution field.
- **Historical evidence:** completed receipts can contribute evidence for provider performance without reducing trust to editable ratings.
- **Machine-readable discovery:** agents can filter structured attributes such as capability, region, capacity, price, and required performance.

Arkiv is not proposed as storage for everything. Private prompts, secrets, private customer data, large files, raw service outputs, and sensitive execution payloads remain off Arkiv. Read [Arkiv's role and the counterfactual](docs/ARKIV.md) for the intended boundary.

## Why x402

x402 is the proposed payment and settlement rail used when a buyer actually pays a provider. It is not Machina's database, marketplace state, or coordination engine. Machina's role is to make the decision and evidence around a service transaction more legible; settlement remains a distinct concern.

## Architecture overview

Machina is organized as three conceptual layers:

1. **Machina Market** — discovery, temporary offers, requests, RFQs, quote comparison, provider selection, coordination, and execution evidence.
2. **Arkiv** — queryable market entities, time-scoped availability, provenance, receipts, and potential reputation evidence.
3. **x402** — payment and settlement when a provider is selected.

The service itself executes outside the Arkiv market-state boundary. [Architecture details](docs/ARCHITECTURE.md) and the [proposed data model](docs/DATA_MODEL.md) describe this separation.

<!-- Future visuals: assets/market-flow.png and assets/machina-console.png -->

## Example interaction

A buyer needs OCR for 10,000 EU documents at at least 99% accuracy with a budget no greater than $8. It can compare temporary offers against those constraints. In the illustrative scenario, Provider C is selected because it satisfies the accuracy threshold; the buyer would then settle with x402, the provider would execute the service, and a receipt could preserve agreed evidence. The scenario is illustrative only. See [the OCR market example](examples/ocr-market/README.md).

## Current status

**Concept / Architecture.** The repository deliberately establishes a product thesis, proposed architecture, conceptual data model, queries, lifecycle, and implementation boundaries. It does not yet implement a marketplace application, database, contract, SDK, payment integration, agent interface, or service integration.

## Roadmap

The work is planned in phases, beginning with validation of the concept and proposed market state, followed by a narrow market-state prototype. See the full [planned roadmap](docs/ROADMAP.md).

## Repository structure

```text
.
├── app/                 # Future application boundary; no implementation yet
├── assets/              # Documentation for planned visual asset locations
├── docs/                # Product and architecture documentation
├── examples/ocr-market/ # Illustrative coordination scenario
└── packages/            # Future shared-package boundary; no implementation yet
```

## Contributing

Machina is at the concept and architecture stage. Contributions should keep implementation claims explicit and distinguish proposals from shipped behavior. Before proposing implementation, start with the relevant documentation and validate the lifecycle, data model, query requirements, privacy boundary, and settlement boundary.

## License

This repository is licensed under the [MIT License](LICENSE).
