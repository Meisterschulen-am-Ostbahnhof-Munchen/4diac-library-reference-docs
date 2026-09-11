# A2X_AUI_DEMUX_4_UNGATED

![A2X_AUI_DEMUX_4_UNGATED](./A2X_AUI_DEMUX_4_UNGATED.svg)

* * * * * * * * * *

## Introduction

The `A2X_AUI_DEMUX_4_UNGATED` function block is a generic one-to-four demultiplexer based on Eclipse 4diac adapters. It receives a value via the `IN` adapter and an index via the `K` adapter, and forwards the input value to one of four output adapters (`OUT1`...`OUT4`).

It is the ungated version of `A2X_AUI_DEMUX_4`: it performs no change detection and forwards every newly computed result unconditionally. This makes it suitable for downstream consumers that require a periodic data cadence independent of whether the sampled value has changed, e.g. derivative or frequency calculations.

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

| Event | Type | Comment |
|-------|------|---------|
| `CNF` | Event | Confirmation of Set Index K. |

### **Data Inputs**

None. All data is transferred through adapters.

### **Data Outputs**

None. All data is transferred through adapters.

### **Adapters**

| Adapter | Direction | Type | Comment |
|---------|-----------|------|---------|
| `IN` | Socket | `adapter::types::unidirectional::A2X` | Input value to demultiplex. |
| `K` | Socket | `adapter::types::unidirectional::AUI` | Index; selects the active output. |
| `OUT1` | Plug | `adapter::types::unidirectional::A2X` | Output value 1, selected when `K = 0`. |
| `OUT2` | Plug | `adapter::types::unidirectional::A2X` | Output value 2, selected when `K = 1`. |
| `OUT3` | Plug | `adapter::types::unidirectional::A2X` | Output value 3, selected when `K = 2`. |
| `OUT4` | Plug | `adapter::types::unidirectional::A2X` | Output value 4, selected when `K = 3`. |

## Functionality

`A2X_AUI_DEMUX_4_UNGATED` implements a 1-to-4 demultiplexing routing logic:

1. The value to distribute is available on the `IN` adapter.
2. The selection index is available on the `K` adapter.
3. Depending on `K`, the value is routed to `OUT1`, `OUT2`, `OUT3`, or `OUT4`.
4. The `CNF` event confirms that the index Set operation has been accepted.

The term `UNGATED` indicates that the FB does not suppress repeated values. While a normal gated demultiplexer would compare the current value with the previous one and only forward a result when a change is detected, this variant always passes the newly calculated/received value through. Therefore, each input update triggers an output update, and the output cadence follows the input cadence.

## Technical Features

- IEC 61499-2 compliant generic function block.
- Generic class name: `GEN_A2X_AUI_DEMUX`.
- Adapter package: `adapter::selection::unidirectional`.
- Four unidirectional `A2X` output plugs.
- Unidirectional `A2X` input socket for the value to demultiplex.
- Unidirectional `AUI` input socket for the selection index.
- No explicit data inputs or data outputs; all payload data crosses the adapter interface.
- No change-detection state or old-value storage.
- Confirmation output event `CNF`.

## State Overview

The FB type itself does not define an internal ECC state machine. Since it is ungated, it does not keep a memory of the last forwarded value and does not compare values. The only relevant selection state is the current index `K`. Once a new index is applied, subsequent input values are forwarded to the newly selected output. With respect to data content, the FB behaves statelessly.

## Application Scenarios

- **Periodic derivative calculation:** A derivative or rate-of-change consumer needs an update in every scan/cycle, even if the measured value stayed constant. The ungated demultiplexer ensures the required cadence.
- **Frequency measurement:** Frequency calculations often rely on time stamps and event counts. Dropping unchanged values would break the calculation; unconditional forwarding prevents this.
- **Value distribution:** One sensor/calculation result can be routed to one of four consumers, selected by a controller or configuration index.
- **Streaming and telemetry:** When a periodic output stream must be maintained towards a selected sub-system, this FB can provide the required steady flow.

## Comparison with Similar Blocks

| Block | Behavior |
|-------|----------|
| `A2X_AUI_DEMUX_4` | Gated demultiplexer. It performs change detection and only forwards new/changed values. |
| `A2X_AUI_DEMUX_4_UNGATED` | Ungated demultiplexer. It forwards every newly available value regardless of whether the content changed. |

The main difference is therefore the presence or absence of value-change filtering. The ungated variant is preferred when downstream processing cannot miss an update cycle. The gated variant is preferred when output bandwidth should be minimized and consumers only need to react to actual value changes.

## Conclusion

`A2X_AUI_DEMUX_4_UNGATED` is a flexible generic 1-to-4 demultiplexer built on unidirectional adapters. It combines a simple index-based output selection with an unconditional forwarding policy. By omitting change detection, it guarantees that every newly computed result reaches the selected consumer, making it particularly suitable for time-sensitive and cadence-dependent calculations such as derivatives and frequency measurements.
