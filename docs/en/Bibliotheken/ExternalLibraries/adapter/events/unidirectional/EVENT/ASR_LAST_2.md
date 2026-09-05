# ASR_LAST_2

![ASR_LAST_2](ASR_LAST_2.svg)

## Introduction

The ASR_LAST_2 function block merges two ASR adapter signals (a set/reset event adapter, no payload data) into one shared output (**OUT**). As with [`AE_LAST_2`](AE_LAST_2.md) there is no data value to arbitrate - here, however, two independent event pairs are merged: SET and RESET, each on its own.

## Interface Structure

### **Adapters**

**Input adapters (Sockets):**

- **IN1**: ASR adapter (unidirectional) - first set/reset source
- **IN2**: ASR adapter (unidirectional) - second set/reset source

**Output adapter (Plug):**

- **OUT**: ASR adapter (unidirectional) - merged set/reset signal

## How it works

Like AE_LAST_2, ASR_LAST_2 is a composite FB with no ECC: both SET events are wired via event connections to `OUT.SET` (`IN1.SET → OUT.SET`, `IN2.SET → OUT.SET`), and independently both RESET events to `OUT.RESET` (`IN1.RESET → OUT.RESET`, `IN2.RESET → OUT.RESET`). SET and RESET are therefore merged separately - a SET from IN1 does not exclude a simultaneous RESET from IN2; both events pass through independently.

## Technical Details

- Pure event fan-in for two independent event pairs (SET, RESET), no ECC/algorithm needed
- Relies on the same IEC 61499 property as AE_LAST_2: multiple event sources into one shared destination
- No signal delay: every event is passed through in the same cycle it arrives
- The SET and RESET paths are fully independent of each other - no priority between them at this block's level

## Application Scenarios

- Merging two equally-valid set/reset sources (e.g. two operator stations allowed to drive the same SR logic) into one shared ASR adapter path
- Simplifying networks that would otherwise need four separate event connections (2× SET, 2× RESET) to the same destination

## ⚖️ Comparison with Similar Blocks

ASR_LAST_2 is the set/reset variant of the pure event fan-in, structurally identical to [`AE_LAST_2`](AE_LAST_2.md), just with two event pairs instead of one. For the data-carrying base variant, see [`AX_LAST_2`](../BOOL/AX_LAST_2.md).
