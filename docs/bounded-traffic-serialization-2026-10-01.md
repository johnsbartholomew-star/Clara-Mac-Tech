# Bounded traffic and serialization qualification — 2026-10-01

## Preconditions

- mac_local_admission = ADMIT_LIGHT_ONLY.
- No heavy local process.
- Canonical Bridge wake state = NO_PENDING_STATE.
- Secure MCP tunnel PID 35954 healthy.
- Tunnel FD baseline = 23.
- Current ceiling unchanged: two concurrent light local jobs maximum.

## Mixed two-wide canary

Five sequential rounds were run. Each round issued exactly two concurrent read-only requests:
- local.status
- bridge_wake_eligibility

All 10 requests completed successfully.

Observed round wall times:
- 797 ms
- 450 ms
- 647 ms
- 753 ms
- 1661 ms

The slower final round did not produce a resource-pressure transition.

After the canary:
- tunnel FD count = 23 (23 -> 23);
- admission remained ADMIT_LIGHT_ONLY;
- memory free ~= 48%;
- swap growth = 0 MB/min;
- no heavy process was reported.

## Same-capability concurrency check

Three rounds each submitted exactly two local.status calls concurrently.

Executor timestamps showed zero execution overlap in all three rounds:

Round 1:
- A 1790896405.402408 -> 1790896405.467031
- B 1790896405.530699 -> 1790896405.569867

Round 2:
- A 1790896406.189495 -> 1790896406.245467
- B 1790896406.324251 -> 1790896406.371666

Round 3:
- A 1790896406.992788 -> 1790896407.054163
- B 1790896407.110638 -> 1790896407.150945

This is direct evidence that the sampled local MCP path serialized these same-capability requests rather than fanning them out concurrently.

Post-check:
- tunnel FD count = 23;
- admission = ADMIT_LIGHT_ONLY;
- memory free ~= 49%;
- swap growth = 0 MB/min.

## Conclusion

The current two-light-job ceiling remains appropriate. Provider request concurrency did not create uncontrolled local process/execution fan-out in this sample. Keep queue/backpressure behavior; do not raise concurrency based on this result.

This is bounded qualification evidence, not proof of unlimited traffic capacity.
