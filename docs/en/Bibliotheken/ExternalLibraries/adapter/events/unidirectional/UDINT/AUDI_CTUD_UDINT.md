# AUDI_CTUD_UDINT (Adapter-Based Up/Down Counter)

## Introduction

The `AUDI_CTUD_UDINT` is an event-driven up/down counter for unsigned 32-bit integers (UDINT), specifically designed for integration into adapter-based systems. It utilizes the `AUDI` adapter for passing the counter value and the default value, enabling a clean separation of event and data flow.
![AUDI_CTUD_UDINT](AUDI_CTUD_UDINT.svg)

## Interface Structure

![AUDI_CTUD_UDINT_ecc](./AUDI_CTUD_UDINT_ecc.svg)

### **Event Inputs**

- **CU**: `Event` - Increments the value by one (`Count Up`; only if `CV.D1` < 4294967295).
- **CD**: `Event` - Decrements the value by one (`Count Down`; only if `CV.D1` > 0).
- **R**: `Event` - Resets the counter to zero (`Reset`).
- **LD**: `Event` - Loads the counter value `CV.D1` with the preset value `PV.D1` (`Load CV with PV`).

### **Event Outputs**

- **CUO**: `Event` - Confirmation of `CU` (`Count Up Output`).
- **CDO**: `Event` - Confirmation of `CD` (`Count Down Output`).
- **RO**: `Event` - Confirmation of `R` (`Reset Output`).
- **LDO**: `Event` - Confirmation of `LD` (`Load Output`).

### **Output Variables**

No classic data outputs; the status signals `QU` and `QD` are provided as AX adapters (plugs), see the Adapter section.

### **Adapter**

- **QU** (Plug): `AX` - `TRUE` when the counter value (`CV.D1`) is greater than or equal to the preset value (`PV.D1`). The event `QU.E1` is only fired when `QU` changes.
- **QD** (Plug): `AX` - `TRUE` if the counter value (`CV.D1`) is less than or equal to zero. The event `QD.E1` is only fired when `QD` changes.
- **CV** (Plug): `AUDI` - The current counter value (`Counter Value`).
- **PV** (Socket): `AUDI` - The default value (`Preset Value`) against which the counter is checked (for `QU`).

## Functionality

The counter reacts to the event inputs `CU`, `CD`, `R` and `LD`. A `CU` event increments `CV.D1` by 1, a `CD` event decrements `CV.D1` by 1. A `R` event resets `CV.D1` to 0, a `LD` event sets `CV.D1` to `PV.D1`.

When an event (`PV.E1`) arrives at the `PV` adapter, only the status signals `QU`/`QD` are re-evaluated; the counter value `CV.D1` is left unchanged.

The counter value is output via the `CV` adapter as the `AUDI` signal. After each counting action, `CUO`, `CDO`, `RO` or `LDO` confirms the operation and `CV.E1` signals the value change; `QU.E1`/`QD.E1` are only fired when the respective status changes (on-change triggering).

## Technical Features

✔ **Adapter-based**: Seamless integration into AX systems.

✔ **Event-driven**: No cyclic calls required.

✔ **UDINT-based**: Supports the full range of unsigned 32-bit integers.

✔ **On-change triggering**: `QU.E1` and `QD.E1` only fire when `QU` or `QD` changes; `PV.E1` re-evaluates both statuses.

## Application Scenarios

- **Piece Counting**: Counting objects in production lines.
- **Operating Hour Counter**: Recording operating times (in combination with timers).
- **Position Monitoring**: Simple position counters in handling systems.

## 🛠️ Related exercises

- [Uebung_009_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_009_AX/)
- [Uebung_083_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_083_AX/)

---

### 🌐 Matching topic subpages on ms-muc-docs.de

- [🌐 E_CTU Event Counter block on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/event-function-blocks/e_ctu/)
