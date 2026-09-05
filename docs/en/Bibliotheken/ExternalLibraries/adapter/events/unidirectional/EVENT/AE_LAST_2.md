# AE_LAST_2

![AE_LAST_2](AE_LAST_2.svg)

## Introduction

The AE_LAST_2 function block merges two pure event adapter signals (**AE**, no payload data) into one shared output (**OUT**). Since AE carries no data value at all, there is nothing to arbitrate when "merging" other than the event itself - the most recently arriving event "wins" in the sense that it (like any other) is passed through to OUT unchanged.

## Interface Structure

### **Adapters**

**Input adapters (Sockets):**

- **IN1**: AE adapter (unidirectional) - first event source
- **IN2**: AE adapter (unidirectional) - second event source

**Output adapter (Plug):**

- **OUT**: AE adapter (unidirectional) - merged event

## How it works

Unlike the data-carrying LAST_2 variants (e.g. [`AX_LAST_2`](../BOOL/AX_LAST_2.md)), AE_LAST_2 is not a Basic FB with an ECC but a composite FB: both input events are wired directly, via event connections, to the same output event (`IN1.E1 → OUT.E1` and `IN2.E1 → OUT.E1`). This is legal in IEC 61499 because event connections (unlike data connections) allow multiple sources into one shared destination. Since AE transports no payload, no ECC is needed to determine "the last value" - every event is simply passed through, regardless of which socket it came from.

## Technical Details

- Pure event fan-in, no ECC/algorithm needed
- Relies on the IEC 61499 property that multiple event sources may be connected to one shared destination
- No signal delay: every event is passed through in the same cycle it arrives
- Structurally the simplest LAST_2 variant, since there is no data to compare/copy

## Application Scenarios

- Merging two equally-valid event sources (e.g. two push buttons meant to trigger the same action) into one shared AE adapter path
- Simplifying networks that would otherwise need two separate event connections to the same destination

## ⚖️ Comparison with Similar Blocks

AE_LAST_2 is the pure-event variant of the generic LAST_2 pattern (see [`AX_LAST_2`](../BOOL/AX_LAST_2.md) for the data-carrying variant). For the special case of the Set/Reset event adapter, see [`ASR_LAST_2`](ASR_LAST_2.md), which merges two separate event pairs (SET, RESET).
