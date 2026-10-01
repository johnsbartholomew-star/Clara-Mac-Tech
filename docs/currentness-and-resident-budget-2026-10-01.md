# Mac Tech currentness and resident-budget note — 2026-10-01

## Verified corrections

- Mac health reporting now derives the Secure MCP restart target from the persisted tunnel profile rather than stale managed-runtime bookkeeping.
- Capability discovery now matches the live bounded Bridge ingress: read capabilities plus exactly three approved Bridge mutations. Restart-safe coordinator reconciliation remains outside the promoted surface.
- Remote Desktop Commander remains an on-demand repair/bootstrap fallback and is not resident after the qualification window.
- Legacy Chrome remote-debugging is classified as retired diagnostic residue, not current Command Center transport. Cleanup should occur only on a safe normal-browser restart; normal user browser state must not be destroyed to remove it.

## Resident-process budget

Secure MCP remains lightweight and resident.

Docker Desktop is not generic Command Bridge infrastructure. Current durable evidence ties the accepted Docker runtime binding to a specialized frozen local capability with exact-image attestation, network disabled, read-only execution boundaries, and no automatic retry. Because that capability is accepted, Docker must not be removed or reconfigured merely as cleanup.

The next efficiency hypothesis is to determine whether Docker can be on-demand rather than continuously warm without weakening the accepted capability or recovery semantics. This requires qualification before any residency or memory-allocation change.

## Operating rule

Prefer provider-side computation and ephemeral local work. Optimize resident services only when provenance, authority, and recovery behavior are proven.
