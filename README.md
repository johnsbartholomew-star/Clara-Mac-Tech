# Clara Mac Tech

Durable engineering home for the Mac-side infrastructure that supports John's Command Center.

## Mission

Keep the Mac boring: reachable, observable, bounded, recoverable, and lightweight while AI workers remain provider/cloud-side.

Target shape:

```
provider-side workers
        ↓
canonical Command Bridge
        ↓
Secure MCP ingress
        ↓
bounded Mac capabilities
```

## Operating rules

- Queue over spawn.
- Event over poll.
- Bounded tools over generic shell.
- On-demand over resident processes.
- Bridge authority over ad-hoc automation.
- The Mac is infrastructure, not the worker host.
- Desktop/browser automation is exceptional, not routine transport.
- Recovery paths must fail closed when an effect may have occurred.

## Repository scope

This repository may contain sanitized Mac-Tech source, tests, runbooks, architecture decisions, qualification methodology, and non-sensitive engineering evidence.

It must **not** contain credentials, tokens, cookies, private authentication material, personal files, sensitive machine identifiers, private infrastructure configuration, or raw telemetry that exposes John's environment.

Operational truth remains in the canonical Command Bridge and live Mac telemetry; this repository is an engineering record, not a second control plane.

## Current focus

1. Browser/automation residue provenance and cleanup only when ownership is proven.
2. Mac Doctor and capability-discovery currentness.
3. Bounded traffic/backpressure qualification.
4. Reliable emergency repair access without making it the production transport.

