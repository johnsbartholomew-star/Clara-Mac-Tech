# MCP Consolidation Ledger — Initial

Date: 2026-10-02

## Observed topology

The long-lived local Codex app-server was observed with 66 direct children totaling roughly 166 MiB RSS.

Repeated families included:
- 7 Bridge MCP children;
- 7 desktop-helper children;
- 7 generic Node MCP servers;
- 14 Computer Use event/history clients;
- 30 CUA runtime/REPL processes.

These are app-server/session children, not independent system daemons.

The counts do not prove each child is unnecessary.

## Consolidation classification

| Family | Initial classification | Rationale |
|---|---|---|
| Qualified Secure MCP ingress | WORKER_FACING / ACTIVE | Existing stable hosted-to-Mac entry |
| Per-session Bridge MCP instances | CONSOLIDATION_CANDIDATE | Same semantic family repeated per session |
| Per-session desktop helpers | BACKEND_ONLY / CONSOLIDATION_CANDIDATE | Workers should request desktop capability, not select helper topology |
| Generic Node MCP children | UNKNOWN / AUDIT | Need plugin ownership mapping |
| Computer Use clients | PROVIDER_MANAGED / OBSERVE | May be session-required; do not kill based on count |
| CUA runtimes | PROVIDER_MANAGED / OBSERVE | Need lifecycle evidence |
| RDC | FALLBACK / MIGRATION | Broad coverage but not desired routine control plane |

## Direction

The target is not “one process.”

The target is:

**one coherent worker-facing capability contract, with specialized backend implementations hidden whenever safe.**

Backend MCPs may remain when technically justified.

A new gateway component is justified only if it reduces total worker-facing and authority complexity and supports migration/retirement of overlapping front doors.
