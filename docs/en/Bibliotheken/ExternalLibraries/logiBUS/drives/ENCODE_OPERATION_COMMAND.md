# ENCODE_OPERATION_COMMAND

![ENCODE_OPERATION_COMMAND](./ENCODE_OPERATION_COMMAND.svg)

* * * * * * * * * *

## Introduction

`ENCODE_OPERATION_COMMAND` builds the Operation Command word for a VFD/drive from `RUN`/`DIRECTION`, or from an explicit Fault Reset event. The block is generic for any drive whose control word follows the common Run/Direction/Stop-plus-Fault-Reset pattern (Modbus, CANopen PZD1, Profibus PPO, etc.) - all concrete command codes are supplied by the caller via Parameter, so the block itself carries no brand-specific knowledge, only the encoding shape.

The name `Operation Command` (rather than `Control Word`) is deliberate: `Control Word` is CANopen-specific terminology, whereas `Operation Command` is the vendor-neutral term used by e.g. the DELIXI H300 manual.

## Interface Structure

### **Event Inputs**

- **`INIT`**: Initialization - sets `OPERATION_COMMAND` to `OC_NO_COMMAND` once.
- **`REQ`**: Re-evaluate `RUN`/`DIRECTION` and encode `OPERATION_COMMAND` accordingly.
- **`FAULT_RESET`**: Request the Fault Reset command code, independent of the current `RUN`/`DIRECTION` state.

### **Event Outputs**

- **`INITO`**: Initialization confirmation.
- **`CNF`**: `OPERATION_COMMAND` updated.

### **Data Inputs**

- **`RUN`** (BOOL): Run command.
- **`DIRECTION`** (BOOL): `FALSE`=Forward, `TRUE`=Reverse.
- **`OC_NO_COMMAND`** (INT): Command code for "no command"/safe default (drive-specific, via Parameter).
- **`OC_FORWARD`** (INT): Command code for Forward Run (drive-specific, via Parameter).
- **`OC_REVERSE`** (INT): Command code for Reverse Run (drive-specific, via Parameter).
- **`OC_DECEL_STOP`** (INT): Command code for Decelerated Stop (drive-specific, via Parameter).
- **`OC_FAULT_RESET`** (INT): Command code for Fault Reset (drive-specific, via Parameter).

### **Data Outputs**

- **`OPERATION_COMMAND`** (INT): Encoded Operation Command word (target register content).

## Functionality

The block has no real state machine (no `<ECC>` with transition conditions) - each event independently triggers exactly one algorithm:

1. **`INIT`**: Sets `OPERATION_COMMAND := OC_NO_COMMAND` before the first `REQ`/`FAULT_RESET` arrives.
2. **`REQ`**: If `RUN = TRUE`, encodes `OC_FORWARD` or `OC_REVERSE` depending on `DIRECTION`; if `RUN = FALSE`, encodes `OC_DECEL_STOP`.
3. **`FAULT_RESET`**: Overwrites `OPERATION_COMMAND` once with `OC_FAULT_RESET`, independent of the current `RUN`/`DIRECTION` state - the next `REQ` encodes normally again afterwards.

## Technical Features

- **All command codes are parameters, no hardcoded knowledge.** The block itself knows no drive-specific numeric values; `OC_*` are supplied by the caller via `Parameter`.
- **Renamed in version 2.0**: Formerly `ENCODE_CONTROLWORD`/`CW_*`/`CONTROL_WORD`, since "Control Word" is CANopen-specific terminology. Semantics unchanged, only names generalized.
- **Generalized from `H300_ENCODE_CONTROLWORD`** (version 1.0): Originally written for one specific drive type, then generalized to the generic Run/Direction/Stop-plus-Fault-Reset pattern.

## Application Scenarios

- Driving a VFD/drive over a fieldbus control word (Modbus holding register, CANopen PZD1, Profibus PPO) where Run/Direction/Stop and an explicit Fault Reset are encoded in a single word.
- Generic block for multiple drive types: only the `OC_*` parameters need adjusting per drive, the encoding logic stays the same.

## Conclusion

`ENCODE_OPERATION_COMMAND` encapsulates the common Run/Direction/Stop-plus-Fault-Reset encoding for VFD control words in a single, vendor-neutral block - all concrete command codes come from outside, the block itself carries no drive-specific knowledge.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
