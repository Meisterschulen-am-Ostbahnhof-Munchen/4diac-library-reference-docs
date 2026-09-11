# AR_TO_AD_NUM

![AR_TO_AD_NUM](./AR_TO_AD_NUM.svg)

* * * * * * * * * *
## Introduction

`AR_TO_AD_NUM` is a composite function block that converts a REAL adapter input into a DWORD adapter output using a numerically correct conversion path. Instead of reinterpreting the REAL value as its IEEE 754 bit pattern, it first converts REAL to UDINT and then moves the resulting unsigned 32-bit integer into a DWORD. This makes it suitable for applications where a numeric setpoint such as `REAL#50.0` should become `DWORD#50` rather than the raw floating-point bit pattern of 50.0.

The block is a drop-in-compatible alternative to `AR_TO_AD` for adapter-based connections that require numeric REAL-to-DWORD conversion, and it forms the counterpart to `AD_TO_AR_NUM` for the reverse direction.

## Interface Structure

### **Event Inputs**

None.  
`AR_TO_AD_NUM` has no directly exposed event inputs. The input event is received through the `AR_IN` adapter.

### **Event Outputs**

None.  
The output event is delivered through the `AD_OUT` adapter.

### **Data Inputs**

None.  
The input data is received through the `AR_IN` adapter.

### **Data Outputs**

None.  
The output data is delivered through the `AD_OUT` adapter.

### **Adapters**

| Role | Name | Type | Description |
|------|------|------|-------------|
| Socket | `AR_IN` | `adapter::types::unidirectional::AR` | REAL adapter input. Supplies the REAL value on `AR_IN.D1` and the corresponding event on `AR_IN.E1`. |
| Plug | `AD_OUT` | `adapter::types::unidirectional::AD` | DWORD adapter output. Provides the converted DWORD value on `AD_OUT.D1` and the completion event on `AD_OUT.E1`. |

## Functionality

The internal network consists of two IEC 61131-2 conversion function blocks:

1. `F_REAL_TO_UDINT` — performs a real numeric conversion from REAL to UDINT.
2. `F_UDINT_TO_DWORD` — transfers the UDINT bit pattern to DWORD.

When an event arrives through `AR_IN.E1`, the REAL value from `AR_IN.D1` is passed to `ToUDINT.IN`. The `F_REAL_TO_UDINT` block converts the value and confirms with `CNF`. This confirmation triggers `ToDWORD.REQ`, passing the UDINT result as input. `F_UDINT_TO_DWORD` then provides the equivalent DWORD representation and confirms with `CNF`, which causes `AD_OUT.E1` to be issued with the result available on `AD_OUT.D1`.

This two-step conversion chain guarantees that the resulting DWORD carries the same numeric value as the original REAL input, rather than the raw IEEE 754 representation of that value.

## Technical Features

- Composite FB implemented entirely through an internal function block network.
- Uses standard IEC 61131 conversion function blocks.
- Converts REAL to DWORD through a UDINT intermediate step.
- Avoids the IEEE 754 bit-reinterpretation behavior of a direct REAL-to-DWORD conversion.
- Unidirectional event flow: input adapter event triggers the conversion pipeline and produces one output adapter event.
- Uses the same adapter types as `AR_TO_AD`, allowing it to be used as a drop-in replacement in adapter-based networks.
- No custom state machine or internal algorithms are required.

## State Overview

`AR_TO_AD_NUM` has no explicitly defined ECC state machine because it is a composite function block. Its behavior can be described as a three-stage request/confirmation pipeline:

| Stage | Active Element | Description |
|-------|----------------|-------------|
| 1 | `ToUDINT` | `AR_IN.E1` triggers `ToUDINT.REQ`; `AR_IN.D1` is passed as the REAL input. |
| 2 | `ToDWORD` | `ToUDINT.CNF` triggers `ToDWORD.REQ`; the converted UDINT value is passed to `ToDWORD.IN`. |
| 3 | `AD_OUT` | `ToDWORD.CNF` triggers `AD_OUT.E1`; the resulting DWORD value is placed on `AD_OUT.D1`. |

The block is idle until an event arrives from the connected REAL adapter.

## Application Scenarios

`AR_TO_AD_NUM` is useful wherever a REAL number must be delivered as the same numeric value in DWORD format, particularly when the DWORD is interpreted by a device or driver as an integer-like value.

Typical use cases include:

- **PWM duty cycle output**: A controller computes a duty cycle as `REAL#50.0` and must send `DWORD#50` to a PWM output such as `logiBUS_QDA_PWM`.
- **Numeric setpoint conversion**: A REAL setpoint from an adapter-based input must be converted into a DWORD value for a downstream integer-oriented interface.
- **Replacing manual conversion chains**: `AR_TO_AD_NUM` packages the equivalent of `AR_TO_AUDI` followed by `AUDI_TO_AD` into a single function block.
- **Interfacing with unidirectional adapters**: The block is designed for use in the `adapter::conversion::unidirectional` package and fits into adapter-based data flow architectures.

For the opposite direction, `AD_TO_AR_NUM` provides the corresponding numeric DWORD-to-REAL conversion.

## Comparison with Similar Blocks

| Approach | Conversion Semantics | Recommended Use |
|----------|----------------------|-----------------|
| `AR_TO_AD_NUM` | REAL → UDINT → DWORD; numeric value is preserved | Numeric setpoints, PWM duty cycles, integer-oriented DWORD outputs |
| `AR_TO_AD` | REAL → DWORD using IEEE 754 bit reinterpretation | Serializing a REAL value as its floating-point bit pattern for transport to a matching DWORD-to-REAL block |
| Manual `AR_TO_AUDI` + `AUDI_TO_AD` | Same numeric semantics as `AR_TO_AD_NUM`, but requires two separate blocks and explicit wiring | When a conversion chain must be made visible or extended in the network |
| `AD_TO_AR_NUM` | DWORD → UDINT → REAL; reverse numeric conversion | Converting a numeric DWORD setpoint back into a REAL value |

The key difference is whether the REAL value is treated as a number or as a bit pattern. `AR_TO_AD_NUM` always preserves the numeric value, while a direct REAL-to-DWORD conversion preserves the IEEE 754 bit layout.

## Conclusion

`AR_TO_AD_NUM` provides a safe and semantically correct way to convert a REAL adapter value into a DWORD adapter value. By routing the conversion through UDINT, it ensures that a numeric value such as `REAL#50.0` becomes `DWORD#50` and not the raw binary representation of the REAL number. This makes it particularly valuable in automation applications where DWORD values are used as setpoints, duty cycles, or other integer-interpreted data.