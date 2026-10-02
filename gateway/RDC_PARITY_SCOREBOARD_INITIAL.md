# RDC parity scoreboard — initial

Observed 2026-10-02.

| Capability family | Historical RDC evidence | G3 implementation | Current normal exposure | Initial disposition |
|---|---:|---|---|---|
| files.read/list/hash/search | high | implemented | direct Secure MCP | G3 PRIMARY |
| files.write/edit | 2808 writes / 899 edits | implemented with root guard, expected prior digest, atomic write, exact-match edit | Bridge-only local core, not general worker-facing | QUALIFY GATEWAY AUTHORITY |
| directory create | 123 creates | missing dedicated G3 capability | none | BUILD/EXTEND G3 |
| files.move/rename | low observed | missing dedicated G3 capability | none | BUILD/EXTEND G3 |
| metadata | 177 info calls | partial metadata returned by list/read/hash; no normalized dedicated capability | partial | EXTEND |
| process.run/start/read/stop | dominant RDC usage; 9060 starts + 2885 output reads | implemented | Bridge-only local core | QUALIFY GATEWAY AUTHORITY |
| process.interact | 95 interactions | no direct G3 equivalent | none | REQUIREMENT REVIEW / BUILD IF NEEDED |
| process.list/inspect | 58 list calls | process_passport covers exact PID provenance, not full list | direct exact-PID inspect | EXTEND |
| process termination | 69 kill + 65 force terminate | process.stop for task-owned execution | Bridge-only | EXTEND AUTHORITY; avoid generic kill |
| desktop.inspect | active | implemented | direct | G3 PRIMARY |
| desktop.act | existing | implemented navigation-only Gen3 | not promoted | QUALIFY PER ACTION CLASS |
| recovery/break-glass | current | Secure MCP login recovery plus RDC spare key | mixed | RDC FALLBACK until gateway parity |

## Key authority finding

Filesystem mutation handlers are technically strong (approved-root guard, expected prior SHA, atomic replace/fsync, bounded write size, exact edit match count). The missing seam for broad autonomous use is caller/objective/workspace authority, not file safety.

Do not globally expose writes merely by adding names to the fixed ingress allowlist. The unified gateway must bind caller/task/workspace authority to effect class.

## Cloud edge qualification

Disposable direct MCP qualification proved optional caller request_id on local_file_hash works end-to-end and echoes exactly in cc-local-exec receipt while legacy callers retain the default ID. Remaining gap is live/provider schema currentness, not local implementation.
