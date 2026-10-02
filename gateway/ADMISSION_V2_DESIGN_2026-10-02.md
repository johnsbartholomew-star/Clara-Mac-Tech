# Mac Local Admission V2 — Shadow Design

Date: 2026-10-02

## Problem

Admission V1 correctly protects the Mac but treats any sufficiently CPU-heavy process as a machine-wide stop condition.

Real observations showed this can block tiny bounded work even when:
- swap is stable;
- memory headroom remains usable;
- the heavy process belongs to normal user or macOS activity.

## Shadow classifier

Admission V2 is implemented locally in shadow mode only.

Pressure tiers:

### GREEN
Allows:
- ESSENTIAL
- TINY
- LIGHT
- HEAVY
- EXCLUSIVE

subject to existing concurrency limits.

### YELLOW
Allows:
- ESSENTIAL
- TINY
- LIGHT

Queues:
- HEAVY
- EXCLUSIVE

### RED
Allows:
- ESSENTIAL

Queues all other cost classes.

## Current principles

- Stable historical swap alone does not create RED.
- Active swap growth is materially different from allocated historical swap.
- A single heavy process produces YELLOW unless stronger pressure evidence exists.
- Severe low memory or rapid active swap growth can produce RED.
- V1 remains the enforcement authority while V2 is observed and falsified.

## Live comparison

During a real normal-Chrome CPU spike:
- Admission V1 returned QUEUE_LOCAL_WORK.
- Admission V2 shadow returned YELLOW.
- V2 would have allowed tiny/light work while still queueing heavy/exclusive work.

This is the intended reduction in unnecessary human/worker stoppage without weakening genuine pressure protection.
