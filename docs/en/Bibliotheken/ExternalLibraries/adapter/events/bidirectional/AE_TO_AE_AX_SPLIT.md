# AE_TO_AE_AX_SPLIT

![AE_TO_AE_AX_SPLIT](AE_TO_AE_AX_SPLIT.svg)

* * * * * * * * * *

## Introduction

AE_TO_AE_AX_SPLIT is a composite function block that converts an incoming **unidirectional** AE event at its socket `IN` into a **bidirectional** AE_AX signal at plug `OUT`, and additionally mirrors the state (event + bool) reported on `OUT`'s backward channel out through a third, unidirectional `AX_OUT` plug. Unlike the pure passthrough [`AE_AX_AX_SPLIT`](AE_AX_AX_SPLIT.md), `IN` has no backward channel of its own here - so the backward state cannot be relayed back to `IN` and is exposed exclusively through `AX_OUT`.

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

- **IN**: Unidirectional adapter socket of type `adapter::types::unidirectional::AE` (input, no backward channel)
- **OUT**: Bidirectional adapter plug of type `adapter::types::bidirectional::AE_AX` (output, with backward channel)
- **AX_OUT**: Unidirectional adapter plug of type `adapter::types::unidirectional::AX`, mirrors the AE_AX backward channel (state) to the outside

## How it works

1. Every event arriving at `IN.E1` is forwarded to `OUT.E1` unchanged.
2. The backward-channel event `OUT.EI1` (with its associated data `OUT.DI1`), sent by the downstream counterpart via `OUT`, is forwarded exclusively to `AX_OUT.E1`/`AX_OUT.D1` - relaying it back to `IN` is technically impossible, since `IN` is a unidirectional AE adapter with no backward channel.
3. `AX_OUT` is therefore the only place where the state reported by `OUT` becomes visible.

## Technical Details

- Pure event/data connections (`FBNetwork`), no logic or state management of its own
- The forward direction (socket → plug) is a simple 1:1 passthrough
- Converts a unidirectional adapter (`IN`) into a bidirectional one (`OUT`) without needing a backward channel on the input itself
- The backward channel is not duplicated (as in AE_AX_AX_SPLIT), but exposed solely via `AX_OUT`

## State Overview

The function block has no internal state and works statelessly. Every incoming event is forwarded/mirrored immediately.

## Application Scenarios

- Connecting an existing, purely unidirectional AE signal source to a chain that expects a bidirectional AE_AX adapter
- Exposing the state of the downstream AE_AX counterpart via `AX_OUT`, even though the original source (`IN`) itself supports no backward channel

## ⚖️ Comparison with Similar Blocks

Unlike [`AE_AX_AX_SPLIT`](AE_AX_AX_SPLIT.md), which mediates between two already-bidirectional adapters and also mirrors the backward channel back to `IN`, AE_TO_AE_AX_SPLIT converts a unidirectional input into a bidirectional output - the backward channel can therefore only be observed via `AX_OUT`. The set/reset and set/reset/toggle variants [`ASR_TO_ASR_AX_SPLIT`](ASR_TO_ASR_AX_SPLIT.md) and [`ASRT_TO_ASRT_AX_SPLIT`](ASRT_TO_ASRT_AX_SPLIT.md) follow the same pattern with two or three forward events instead of a single one.

## Summary

AE_TO_AE_AX_SPLIT allows a purely unidirectional AE signal source to connect to a bidirectional AE_AX chain and makes its backward channel available in isolation via `AX_OUT`, without requiring the original input itself to support a backward channel.
