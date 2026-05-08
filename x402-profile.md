# x402-signals — x402 profile (placeholder)

`x402-signals` v0.1 ([v0.1.md](v0.1.md)) is written in the presence of
the existing x402 envelope (`PAYMENT-REQUIRED`, `PAYMENT-SIGNATURE`,
`PAYMENT-RESPONSE`, `extensions[bazaar]`) but its three buckets
(`fulfillment_policy`, `refund_policy`, `signals`) are deliberately
asset- and facilitator-agnostic.

This document captures the **x402-specific bindings** — the bits that
only make sense when the underlying settlement layer is x402.

## Status

Placeholder. Bindings for v0.1 are inlined into
[v0.1.md §5 (Wire format)](v0.1.md#5-wire-format) and §8 (Reference
example). A standalone profile document becomes useful only when v0.2
adds bindings for non-x402 protocols (e.g. native Lightning, native
Stripe payment intents) and the cross-protocol gymnastics need a
dedicated home.

Until then, this file exists to reserve the path. v0.2 work to
populate it includes:

- Mapping `original_payer` → EIP-712 recovered signer for `eip155:*`
  networks.
- Mapping `original_payer` → SPV-recovered payer for non-EVM networks
  (no current implementer; speculative).
- How `extensions["x402-signals"]` interacts with `extensions["bazaar"]`
  in a single 402 challenge.
- Facilitator-side observability hooks (settle receipt → fulfillment
  trigger).
- Profile-specific test vectors for facilitator and provider conformance
  suites.

## Cross-references

- [v0.1.md](v0.1.md) — canonical spec.
- [README.md](README.md) — entry point.
