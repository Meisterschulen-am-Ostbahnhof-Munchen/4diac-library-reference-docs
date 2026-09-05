# ASRT_TO_ASRT_AX_SPLIT

![ASRT_TO_ASRT_AX_SPLIT](ASRT_TO_ASRT_AX_SPLIT.svg)

* * * * * * * * * *

## Introduction

ASRT_TO_ASRT_AX_SPLIT is the set/reset/toggle variant of [`ASR_TO_ASR_AX_SPLIT`](ASR_TO_ASR_AX_SPLIT.md): it converts an incoming **unidirectional** ASRT signal (set/reset/toggle, no backward channel) at its socket `IN` into a **bidirectional** ASRT_AX signal at plug `OUT`, and additionally mirrors the state reported on `OUT`'s backward channel out through a third, unidirectional `AX_OUT` plug.

## Interface Structure

### **Event Inputs**

*No direct event inputs - events arrive via the adapter sockets/plugs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

*No data inputs*

### **Data Outputs**

*No data outputs*

### **Adapters**

- **IN**: Unidirectional adapter socket of type `adapter::types::unidirectional::ASRT` (set/reset/toggle input, no backward channel)
- **OUT**: Bidirectional adapter plug of type `adapter::types::bidirectional::ASRT_AX` (set/reset/toggle output, with backward channel)
- **AX_OUT**: Unidirectional adapter plug of type `adapter::types::unidirectional::AX`, mirrors the ASRT_AX backward channel (state) to the outside

## How it works

1. Every event arriving at `IN.SET` is forwarded to `OUT.SET` unchanged, likewise `IN.RESET` to `OUT.RESET` and `IN.TOGGLE` to `OUT.TOGGLE` - all three event paths run independently of each other.
2. The backward-channel event `OUT.EI1` (with its associated data `OUT.DI1`), sent by the downstream counterpart via `OUT`, is forwarded exclusively to `AX_OUT.E1`/`AX_OUT.D1` - relaying it back to `IN` is technically impossible, since `IN` is a unidirectional ASRT adapter with no backward channel.
3. `AX_OUT` is therefore the only place where the state reported by `OUT` becomes visible.

## Technical Details

- Pure event/data connections (`FBNetwork`), no logic or state management of its own
- Three independent forward paths (SET, RESET, TOGGLE), each a simple 1:1 passthrough
- Converts a unidirectional adapter (`IN`) into a bidirectional one (`OUT`) without needing a backward channel on the input itself
- The backward channel is not duplicated, but exposed solely via `AX_OUT`

## State Overview

The function block has no internal state and works statelessly. Every incoming event is forwarded/mirrored immediately.

## Application Scenarios

- Connecting an existing, purely unidirectional ASRT signal source (e.g. a SoftKey with set/reset/toggle behavior) to a chain that expects a bidirectional ASRT_AX adapter
- Exposing the state of the downstream ASRT_AX counterpart via `AX_OUT`, even though the original source (`IN`) itself supports no backward channel

## ⚖️ Comparison with Similar Blocks

Structurally identical to [`ASR_TO_ASR_AX_SPLIT`](ASR_TO_ASR_AX_SPLIT.md), just with one additional third forward event (TOGGLE). For the simpler set/reset variant without toggle, see [`ASR_TO_ASR_AX_SPLIT`](ASR_TO_ASR_AX_SPLIT.md); for the pure event variant without set/reset semantics, see [`AE_TO_AE_AX_SPLIT`](AE_TO_AE_AX_SPLIT.md).

## Summary

ASRT_TO_ASRT_AX_SPLIT allows a purely unidirectional ASRT signal source to connect to a bidirectional ASRT_AX chain and makes its backward channel available in isolation via `AX_OUT`, without requiring the original input itself to support a backward channel.
