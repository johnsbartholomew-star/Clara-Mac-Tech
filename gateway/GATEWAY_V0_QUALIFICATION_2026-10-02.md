# Gateway V0 qualification — 2026-10-02

## Decision

EXTEND canonical Bridge continuation binding + G3 rather than create a second authority database.

Gateway authority carrier:
- opaque canonical continuation_request_id;
- canonical current binding verification;
- non-empty authority and constraint refs;
- positive gateway grant inside the canonical continuation payload;
- granted capability, workspace root and effect class;
- G3 local execution receipt keyed from canonical logical_effect_id.

Self-asserted role names are not accepted as authority.

## V0 implemented

Local prototype: command-center-gateway-v0.

Initial capabilities:
- file.write
- file.edit
- directory.create

G3 gained directory.create as a bounded LOCAL_MUTATION capability.

## Qualification

Gateway acceptance:
- authorized file create: PASS
- exact replay/no second effect: PASS
- stale prior SHA fails closed: PASS
- wrong workspace: DENIED
- ungranted capability: DENIED
- missing authority: DENIED
- stale objective binding: DENIED
- directory create + replay: PASS

Gateway tests: 3/3 PASS.

Existing G3 regression after directory.create: 41/41 PASS.

Production canonical Bridge remained schema 6, queue 0, wake pending 0, orphan leases 0.

## Cloud edge

Provider-visible local_file_hash now exposes optional request_id. Disposable local qualification previously proved exact caller ID echo in cc-local-exec receipt with unchanged hash.

## Process replacement direction

Do NOT simply add python3/shell to the old allowlist.

macOS sandbox-exec was verified on this Mac:
- workspace write allowed;
- outside write denied;
- network can be denied;
- explicit sensitive file reads can be denied while Python remains runnable.

Next build seam is a workspace-sandboxed engineering runner behind the same canonical gateway grant, with bounded argv/timeout/output and durable G3 receipts. This targets the dominant historical RDC process workload without turning the gateway into unrestricted shell.
