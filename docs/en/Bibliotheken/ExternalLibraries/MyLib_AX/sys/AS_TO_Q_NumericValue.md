# AS_TO_Q_NumericValue


![AS_TO_Q_NumericValue_network](./AS_TO_Q_NumericValue_network.svg)

![AS_TO_Q_NumericValue](./AS_TO_Q_NumericValue.svg)

* * * * * * * * * *
## Introduction

`AS_TO_Q_NumericValue` is a reusable subapplication designed to display an AS adapter value (e.g., a step number from a sequencer) on a Visualization Terminal (VT) numeric field. It generically uses an Object ID to select the target field and leverages the `Q_NumericValue_AUDI` function block from the `isobus::UT::Q` library. The block acts as a cleaner interface by accepting an AS adapter input instead of a raw numeric signal, making it suitable for integration into Industrial Automation systems.

## Interface Structure

The subapplication exposes a minimal interface, consisting of one data input and one adapter socket. It has no event inputs, event outputs, or data outputs.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Name       | Type | Initial Value | Comment                |
|------------|------|---------------|------------------------|
| `u16ObjId` | UINT | `ID_NULL`     | Object ID for the VT numeric field (target). |

### **Data Outputs**

None.

### **Adapters**

| Name       | Type                                          | Direction | Comment                         |
|------------|-----------------------------------------------|-----------|---------------------------------|
| `STATE_NR` | `adapter::types::unidirectional::AS`          | Socket    | AS adapter input carrying the value to display. |

## Functionality

The subapplication performs a unidirectional data conversion and transfer:

1. `STATE_NR` (AS adapter) carries the value (e.g., a step number) as an `AS` type.
2. The internal `AS_TO_AUDI` block converts the AS adapter value into an AUDI-compatible data stream (`AUDI_OUT`).
3. The converted AUDI value is then fed into `Q_NumericValue_AUDI` as `u32NewValue`.
4. Simultaneously, the `u16ObjId` input is passed directly to `Q_NumericValue_AUDI` to define which VT numeric field should be updated.

In essence, the block bridges an AS signaling network with a VT display, making the object ID externally configurable without changing internal wiring.

## Technical Features

- **Generic Object ID**: The `u16ObjId` input is fully parametrizable, allowing the same subapplication to control different VT fields without duplication.
- **Reusability**: Designed for reuse across multiple projects or instances; it was extracted from a larger exercise for modularity.
- **Leverages Standard Library Blocks**: Utilizes `isobus::UT::Q::Q_NumericValue_AUDI` and `adapter::conversion::unidirectional::AS_TO_AUDI`, ensuring compatibility with IEC 61499‑2 and the 4diac IDE.
- **Simple Wiring**: Only one adapter input and one data input are required; no internal logic or state variables are exposed.

## State Overview

This subapplication is **stateless** in its own right. It does not maintain any internal state variables or sequencing logic. All data flows are purely combinational from the adapter input and the object ID to the output of the internal `Q_NumericValue_AUDI` block. The functionality is deterministic and does not depend on execution order or history.

## Application Scenarios

Typical use cases include:

- Displaying the current step number of a sequencer on an HMI or VT.
- Indicating the active recipe or status code from a machine as a numeric readout.
- Updating a numeric field on a VT based on a value from an AS‑connected device, without requiring direct signal wiring.
- Serving as a drop‑in replacement in systems that previously used a direct `SINT` input, now adding the flexibility of an AS adapter.

## Comparison with Similar Blocks

The sister block `MyLib::sys::SINT_TO_Q_NumericValue` provides the same functionality but expects a direct `SINT` value as input instead of an AS adapter. The main differences:

- **Input Type**: `SINT_TO_Q_NumericValue` takes a plain `SINT` data signal; `AS_TO_Q_NumericValue` requires an AS adapter.
- **Flexibility**: The AS adapter version is better suited for environments already using AS‑communication patterns, while the SINT version may be simpler for purely data‑oriented connections.
- **Wiring**: The AS variant adds an extra conversion layer (`AS_TO_AUDI`), which is transparent to the user and handled internally.

Choosing one over the other depends on the surrounding system architecture and the availability of AS adapter connections.

## Conclusion

`AS_TO_Q_NumericValue` is a compact, reusable subapplication that effectively translates an AS adapter value into a numeric display on a VT, with an externally configurable object ID. Its stateless design and use of standard library blocks make it straightforward to integrate into IEC 61499‑based projects. By reusing this block, developers can avoid duplicating conversion logic and ensure consistency across automation projects that require numeric visualization of AS signals.