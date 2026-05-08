# x402-signals

A small, machine-readable convention for x402 services to declare
**fulfillment SLA**, **refund policy**, and **operational signals** so
that paying agents can reason about risk, wait, and recovery without
human intervention.

| Document | Status | Last updated |
|---|---|---|
| [v0.1.md](v0.1.md) — full v0.1 DRAFT | DRAFT | 2026-05-08 |
| [x402-profile.md](x402-profile.md) — x402-specific bindings | placeholder | 2026-05-08 |

## TL;DR

A provider that wants to publish x402-signals adds three top-level
fields to its existing `/.well-known/x402` document (or to a 402
challenge `extensions["x402-signals"]` block):

```json
{
  "fulfillment_policy": {
    "mode": "instant",
    "fulfillment_deadline_seconds": 60,
    "status_endpoint": "/api/orders/{orderId}",
    "retry_after_seconds": 5
  },
  "refund_policy": {
    "type": "automatic",
    "triggers": ["fulfillment_failed", "upstream_timeout"],
    "refund_to": "original_payer",
    "refund_endpoint": "/api/refunds",
    "refund_deadline_seconds": 86400,
    "refund_claim_deadline_seconds": null,
    "partial_refunds_supported": true,
    "idempotency_required": true
  },
  "signals": {
    "provider_health": "healthy",
    "agent_action": "pay",
    "last_updated": "2026-05-08T14:30:00Z"
  }
}
```

Every paid `200` response then carries a small object so that lost
responses can be recovered:

```json
{
  "paymentId": "pay_01H8...",
  "orders": [ { "orderId": "ord_01H8a...", "state": "FULFILLED", "result": { ... } } ]
}
```

That's it. No new wire envelope, no required transport, no fee. Read
[v0.1.md](v0.1.md) for the canonical state machine, field-by-field
semantics, status / refund endpoint contracts, security considerations,
and a worked reference example against a real on-chain eSIM purchase.

## Why

x402 settles money in seconds. It does not yet specify what an agent
should expect when an upstream supplier is degraded, when fulfillment
takes longer than the HTTP request, or when a paid call fails after
settlement. Without a convention, every operator invents a private one
and every agent re-implements the same dispute logic. This is the
convention.

## Open process

This is a working DRAFT. Comments, edge cases, missing patterns, and
alternative wordings are welcome.

- File an issue at
  [github.com/sF1nX/x402-signals](https://github.com/sF1nX/x402-signals/issues).
- Mention [@x402station_io](https://x.com/x402station_io) on X.
- Email `hello@x402station.io`.

If you operate an x402 service and want to be the **first
implementer** of v0.1 (publishing the three buckets and exposing
matching status / refund endpoints), we will help you wire it and cite
you explicitly in v0.2. No fee, no commitment. ReloadPI is the
**pilot target** that triggered the draft; the first-implementer slot
is open.

## License

CC0 1.0 — public domain dedication. Take it. Fork it. Improve it.
