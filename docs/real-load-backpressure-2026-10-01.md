# Real-load backpressure observation — 2026-10-01

## Observation

During normal Mac-Tech traffic-readiness work, mac_local_admission changed from ADMIT_LIGHT_ONLY to QUEUE_LOCAL_WORK without synthetic load.

The attributable heavy process was Apple's signed `mediaanalysisd`, observed near 100% CPU. Process provenance was launchd-rooted, executable under the macOS MediaAnalysis private framework, with no TCP listeners or established TCP connections.

At the same time Secure MCP remained reachable and its tunnel process retained 23 file descriptors.

## Response

Mac Tech did not:
- start a traffic test;
- bypass admission;
- spawn another local worker;
- kill or throttle the Apple service;
- restart Secure MCP;
- use RDC as a workaround.

Local work was deferred. A later admission check returned ADMIT_LIGHT_ONLY with no heavy processes, roughly 50% free memory, stable historical swap and zero observed swap growth.

## Conclusion

This is positive operational evidence for the intended backpressure policy under unrelated real Mac load:

`heavy local OS work -> QUEUE_LOCAL_WORK -> local work waits -> pressure clears -> ADMIT_LIGHT_ONLY`.

It does not establish maximum traffic capacity or justify increasing the two-light-job ceiling.
