# Codex / Computer Use FD watch — 2026-10-01

## Current observation

A provider-managed helper fan-out pattern is present under the ChatGPT/Codex processes.

Observed helper topology:
- local Codex app-server: two event-stream/computer-history helper pairs;
- Cloud Codex exec-server: two event-stream/computer-history helper pairs.

All sampled helpers are OpenAI-signed, have no listeners or established TCP connections, and use about 20-21 file descriptors each.

Parent observations during active use:
- local Codex app-server PID 22916: earlier sample ~82 FDs; later sample 139 FDs while helper/plugin session count increased;
- Cloud exec-server PID 22976: 52 FDs in both sampled periods;
- SkyComputerUseService PID 22929: ~174 -> 177 FDs.

## Classification

WATCH / NOT_PROVEN_LEAK.

Do not kill provider-managed helper processes merely because duplicate pairs exist. Multiple pairs can reflect active plugin/session instances.

Escalate toward leak only if:
1. helper/task/session activity settles or declines;
2. the same parent process epoch remains alive;
3. parent FD count continues increasing across later spaced samples; and
4. descriptors do not return toward a stable baseline.

If the parent PID changes, establish a new baseline rather than comparing across process epochs.

This watch is intentionally observational and does not authorize ChatGPT/Codex process cleanup.
