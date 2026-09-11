# AUDI_AUI_MUX_32

![AUDI_AUI_MUX_32](./AUDI_AUI_MUX_32.svg)

* * * * * * * * * *
## Introduction

AUDI_AUI_MUX_32 is a generic function block that implements a 32-channel adapter multiplexer. It selects one of 32 unidirectional AUDI adapter inputs (IN1 to IN32) based on an index value received via a dedicated AUI adapter input (K) and forwards the selected data to the unidirectional AUDI adapter output (OUT). The output is only updated when the value actually changes, meaning the confirmation event is emitted solely upon a real value change, avoiding unnecessary retransmissions.

This block is part of the adapter-based selection pattern and is designed for applications where multiple data sources must be routed to a single consumer under dynamic index control.

## Interface Structure

### **Event Inputs**

The function block has **no event inputs**. All data transfer is handled through adapter connections.

### **Event Outputs**

| Event | Description |
|-------|-------------|
| `CNF` | Confirmation that the set index K has been processed and the output was updated (only issued if an actual value change occurred). |

### **Data Inputs**

There are **no direct data inputs**. All input data is provided through the socket adapters.

### **Data Outputs**

There are **no direct data outputs**. All output data is provided through the plug adapter.

### **Adapters**

| Adapter | Direction | Type | Description |
|---------|-----------|------|-------------|
| `OUT` | Plug | `adapter::types::unidirectional::AUDI` | Output adapter carrying the selected input value. |
| `K` | Socket | `adapter::types::unidirectional::AUI` | Index input (0 to 31) selecting which of the 32 inputs is routed to the output. |
| `IN1` | Socket | `adapter::types::unidirectional::AUDI` | Input value 1 (selected when K = 0). |
| `IN2` | Socket | `adapter::types::unidirectional::AUDI` | Input value 2 (selected when K = 1). |
| `IN3` | Socket | `adapter::types::unidirectional::AUDI` | Input value 3 (selected when K = 2). |
| `IN4` | Socket | `adapter::types::unidirectional::AUDI` | Input value 4 (selected when K = 3). |
| `IN5` | Socket | `adapter::types::unidirectional::AUDI` | Input value 5 (selected when K = 4). |
| `IN6` | Socket | `adapter::types::unidirectional::AUDI` | Input value 6 (selected when K = 5). |
| `IN7` | Socket | `adapter::types::unidirectional::AUDI` | Input value 7 (selected when K = 6). |
| `IN8` | Socket | `adapter::types::unidirectional::AUDI` | Input value 8 (selected when K = 7). |
| `IN9` | Socket | `adapter::types::unidirectional::AUDI` | Input value 9 (selected when K = 8). |
| `IN10` | Socket | `adapter::types::unidirectional::AUDI` | Input value 10 (selected when K = 9). |
| `IN11` | Socket | `adapter::types::unidirectional::AUDI` | Input value 11 (selected when K = 10). |
| `IN12` | Socket | `adapter::types::unidirectional::AUDI` | Input value 12 (selected when K = 11). |
| `IN13` | Socket | `adapter::types::unidirectional::AUDI` | Input value 13 (selected when K = 12). |
| `IN14` | Socket | `adapter::types::unidirectional::AUDI` | Input value 14 (selected when K = 13). |
| `IN15` | Socket | `adapter::types::unidirectional::AUDI` | Input value 15 (selected when K = 14). |
| `IN16` | Socket | `adapter::types::unidirectional::AUDI` | Input value 16 (selected when K = 15). |
| `IN17` | Socket | `adapter::types::unidirectional::AUDI` | Input value 17 (selected when K = 16). |
| `IN18` | Socket | `adapter::types::unidirectional::AUDI` | Input value 18 (selected when K = 17). |
| `IN19` | Socket | `adapter::types::unidirectional::AUDI` | Input value 19 (selected when K = 18). |
| `IN20` | Socket | `adapter::types::unidirectional::AUDI` | Input value 20 (selected when K = 19). |
| `IN21` | Socket | `adapter::types::unidirectional::AUDI` | Input value 21 (selected when K = 20). |
| `IN22` | Socket | `adapter::types::unidirectional::AUDI` | Input value 22 (selected when K = 21). |
| `IN23` | Socket | `adapter::types::unidirectional::AUDI` | Input value 23 (selected when K = 22). |
| `IN24` | Socket | `adapter::types::unidirectional::AUDI` | Input value 24 (selected when K = 23). |
| `IN25` | Socket | `adapter::types::unidirectional::AUDI` | Input value 25 (selected when K = 24). |
| `IN26` | Socket | `adapter::types::unidirectional::AUDI` | Input value 26 (selected when K = 25). |
| `IN27` | Socket | `adapter::types::unidirectional::AUDI` | Input value 27 (selected when K = 26). |
| `IN28` | Socket | `adapter::types::unidirectional::AUDI` | Input value 28 (selected when K = 27). |
| `IN29` | Socket | `adapter::types::unidirectional::AUDI` | Input value 29 (selected when K = 28). |
| `IN30` | Socket | `adapter::types::unidirectional::AUDI` | Input value 30 (selected when K = 29). |
| `IN31` | Socket | `adapter::types::unidirectional::AUDI` | Input value 31 (selected when K = 30). |
| `IN32` | Socket | `adapter::types::unidirectional::AUDI` | Input value 32 (selected when K = 31). |

## Functionality

The block continuously monitors the index value received on the `K` socket. When the index changes (or a new value arrives), the corresponding input adapter data (IN1 to IN32) is routed to the `OUT` adapter. The output value is compared with the previously forwarded value; if a difference is detected, the `CNF` event is emitted. If the value remains identical, no event is generated, preventing unnecessary load on downstream components.

The multiplexing logic is generic – the block is marked as a generic function block (`GEN_AUDI_AUI_MUX`), meaning the actual adapter types and internal behavior are resolved at deployment time.

## Technical Features

- **32-to-1 multiplexing** with unidirectional adapter interfaces.
- **Change-detection** on the output: `CNF` is only triggered when the selected value actually changes.
- **No event inputs** required – the block operates autonomously on incoming adapter data.
- **Index range**: 0 to 31, corresponding to IN1 to IN32.
- **Unidirectional data flow** – all adapters are unidirectional, simplifying the interface.
- **Generic FB type** – uses the eclipse 4diac generic mechanism for late type resolution.

## State Overview

The block does not expose explicit internal states. It can be conceptually described by two implicit operating phases:

1. **Idle / Monitoring** – Waiting for a new index or input value change.
2. **Update** – Forwarding the selected input and evaluating whether the output value differs from the previous one, optionally issuing `CNF`.

No persistent state or mode beyond the current selected value and its cached copy is maintained.

## Application Scenarios

- **Signal routing in agricultural machinery** (as indicated by the source context) where one of many sensors must be connected to a display or controller.
- **Dynamic sensor selection** – choosing between 32 measurement channels based on a control index.
- **Data consolidation** – merging multiple producer adapters into a single consumer path.
- **Event-driven communication** – where downstream components should only be notified when a value really changes, to reduce bus traffic.

## Comparison with Similar Blocks

| Feature | AUDI_AUI_MUX_32 | Conventional Data MUX |
|---------|----------------|-----------------------|
| Interface | Fully adapter-based (AUDI/AUI) | Direct data inputs (ANY) |
| Change detection | Yes, `CNF` only on real change | Typically always raises event |
| Number of channels | 32 | Variable (often 2, 4, 8) |
| Generic type | Yes (generic FB) | Usually typed |
| Event inputs | None (autonomous) | Often explicit trigger event |

Compared to a classic IEC 61499 multiplexer FB (e.g., using data inputs and a selector input), this block is oriented towards adapter-based communication and provides inherent change filtering, making it better suited for distributed, event-driven systems.

## Conclusion

AUDI_AUI_MUX_32 is a powerful and flexible adapter multiplexer for up to 32 unidirectional data sources. Its generic nature and change-detection behavior reduce unnecessary event traffic, while the adapter-based interface integrates seamlessly with modern 4diac IEC 61499 applications. It is especially useful in scenarios where a single consumer must be dynamically connected to a large number of producers.