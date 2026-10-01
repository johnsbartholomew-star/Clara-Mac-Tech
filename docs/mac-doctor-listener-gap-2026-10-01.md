# Mac Doctor gap: listener-based browser residue — 2026-10-01

## Finding

Mac Doctor's current `stale_headless` check searches process command text for strings such as `remote-debugging-port`, `clara-cdp`, and `clara-v5-cdp`.

The current long-lived normal Google Chrome process owns loopback listener `127.0.0.1:9222`, attributable to the retired external Chrome remote-debugging work, while its current process command line does not contain a remote-debugging marker.

Result: Mac Doctor can report `stale_headless.ok=true` while the retired loopback debugging listener still exists.

## Classification

OBSERVABILITY_GAP / NOT_LAUNCH_BLOCKING.

The listener is loopback-only, current Command Center code does not depend on it, and historical currentness records classify external Chrome remote debugging as diagnostic/engineering-only rather than routine transport.

Do not restart or kill John's active Chrome solely to remove it.

## Smallest future repair

During an authorized local-write window, extend Mac Doctor browser-residue observability to inspect listener ownership (at minimum known retired debugging ports/surfaces) rather than relying only on command-line markers. Report provenance without destructive cleanup.

Cleanup of the existing listener should occur only with a safe normal Chrome restart or another ownership-proven lifecycle event.
