# Unified Capability Gateway — first inventory and consolidation evidence

Observed: 2026-10-02

## Current architectural facts

- Secure MCP / Command Center Mac is the qualified hosted ingress.
- G3/local executor currently implements 13 local capability names: local.status; file.list/read/hash/find_literal/write/edit; process.run/start/read/stop; desktop.inspect/act.
- Worker-facing Command Center capability discovery exposes the read/status/scout families directly while file write/edit, process operations and desktop.act remain non-worker-facing/Bridge-only.
- Desktop Commander Remote remains broad break-glass/migration tooling and currently has full-filesystem reach under its own configuration.
- Canonical Bridge remained schema 6 with queue 0 and orphan leases 0 at inventory time.

## Local Codex per-session multiplication

The long-lived local Codex app-server PID 22916 had 66 direct child processes totaling about 170192 KiB RSS in the sampled snapshot.

Classified direct children:
- bridge_mcp: 7 children / 9872 KiB
- desktop_helper: 7 / 12672 KiB
- node_mcp_server: 7 / 19568 KiB
- sky_computer_use_client: 14 / 69136 KiB
- cua_runtime: 30 / 57584 KiB
- other sampled: 1 / 1360 KiB

The seven Bridge MCP and seven desktop-helper processes share the same Codex app-server parent; they are not independent system daemons. This is evidence of per-session/plugin backend multiplication, not proof that each child is individually unnecessary.

The separate qualified Secure MCP path is one tunnel process with one effectful Command Center MCP child.

## First consolidation decision

Do not kill live Codex children during active sessions.

Treat equivalent per-session local capability-server multiplication as a primary gateway consolidation target.

Desired direction: workers consume one governed capability contract; specialized implementations may remain backend-only, but ordinary workers should not need to instantiate or reason about overlapping local MCP topology when a shared qualified ingress can safely serve the capability.

## Cloud edge

The on-disk unified MCP schema already permits optional bounded request_id on local_file_hash (min 1 / max 128), and the wrapper/local executor already carries and echoes request_id. Some hosted sessions still advertise the older path-only schema. The remaining issue is provider/runtime currentness/promotion, not missing local implementation.

## Repair / extend / replace / build decisions so far

- local_file_hash request correlation: REPAIR/currentness, not build.
- G3 local implementation: EXTEND/promote where authority contracts fit.
- RDC routine dependency: REPLACE progressively; retain break-glass until parity/recovery proven.
- per-session overlapping local MCP topology: CONSOLIDATE behind gateway; do not destructively clean live children.
- binary admission: REPLACE with cost-aware graded admission after qualification.
- true gateway component: decision remains open pending fuller surface inventory; build is authorized if existing seams cannot cleanly own routing.
