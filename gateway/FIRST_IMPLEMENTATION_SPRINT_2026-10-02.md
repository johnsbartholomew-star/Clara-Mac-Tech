# Unified Capability Gateway — First Implementation Sprint

Date: 2026-10-02

## Result

The first implementation sprint moved from architecture to working code without changing the qualified production Secure MCP catalog.

### Live schema currentness

The existing provider-facing file-hash capability now advertises and accepts an optional bounded caller request ID. A live hosted call returned the exact caller-supplied ID in the local execution receipt with the expected hash and executor identity.

The Secure MCP tunnel did not restart between the earlier stale-schema observation and this successful live call. Current evidence therefore supports runtime/tool-schema currentness as the earlier mismatch rather than missing Mac-side functionality.

### Gateway V0

A local, non-production Gateway V0 now exists as a thin authority/routing layer.

It does not own a new authority database.

It reads existing Bridge project/task truth in read-only mode and requires:
- active registered project;
- current project revision;
- active task;
- task worker matching the caller;
- caller allowed by project;
- no approval/blocker;
- target path inside the registered task workspace.

Its first normalized capabilities are:
- files.write
- files.edit

The gateway routes those operations to the existing bounded local execution backend.

### Qualification

Disposable tests established:
- governed write: PASS;
- governed edit: PASS;
- identical request replay: same receipt / no duplicate effect;
- request ID reused with different payload: REQUEST_ID_CONFLICT;
- stale prior digest: STALE_FILE;
- outside workspace: DENIED;
- wrong caller: DENIED;
- stale project revision: DENIED;
- simulated crash after effect but before final receipt commit: ACTION_STATE_UNKNOWN;
- replay after uncertain effect: ACTION_STATE_UNKNOWN / reconciliation required.

A disposable MCP-shaped front door exposing one normalized governed execution tool also passed write, replay, and workspace-denial tests. It is not connected to the production tunnel yet.

### Gen3 effect-boundary repair

The stronger Gen3 repair backend was extended so file.write/file.edit participate in durable effect reservation and mark DISPATCHING at the actual filesystem mutation boundary.

Targeted existing regression suite: 18/18 PASS.

### Production impact

Production Bridge state and the qualified Secure MCP 20-tool surface were not expanded during this sprint.

RDC remains migration/break-glass tooling while the gateway replacement path qualifies.
