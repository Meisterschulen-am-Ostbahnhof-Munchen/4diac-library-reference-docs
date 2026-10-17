# AOption_OR_2

![AOption_OR_2](./AOption_OR_2.svg)

![AOption_OR_2_network](./AOption_OR_2_network.svg)

* * * * * * * * * *

## Introduction

`AOption_OR_2` is a composite FB that serves as a **fan-in** (OR combination) for the option adapter [`AOption`](../AOption.md): two option requests at the sockets `IN1` and `IN2` are merged into a single request at the plug `OUT`. `OUT` is active as long as `IN1` **or** `IN2` is active. The block is the counterpart of [`AOption_T_2`](../splitting/AOption_T_2.md).

## Interface Structure

### **Event Inputs**

None (the events run through the adapters).

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `IN1` | `Option::AOption` | Socket | First AOption input. |
| `IN2` | `Option::AOption` | Socket | Second AOption input. |
| `OUT` | `Option::AOption` | Plug | Common output - active as long as `IN1` OR `IN2` is active. |

## Functionality

The internal network contains one FB `OR_QI` of type `iec61131::booleanOperators::OR_BOOL_2`.

Events:

- `IN1.INIT_REQ` -> `OR_QI.REQ` and `IN2.INIT_REQ` -> `OR_QI.REQ`
- `OR_QI.CNF` -> `OUT.INIT_REQ`
- `IN1.INIT_REQ` -> `IN1.INIT_CNF` and `IN2.INIT_REQ` -> `IN2.INIT_CNF`

Data:

- `IN1.QI_OUT` -> `OR_QI.IN1`, `IN2.QI_OUT` -> `OR_QI.IN2`
- `OR_QI.OUT` -> `OUT.QI_OUT`

Each request at `IN1` or `IN2` triggers the OR operation. Its result is passed on to `OUT` as `INIT_REQ` with `QI_OUT`.

## Technical Notes

- **Immediate confirmation:** the inputs' `INIT_CNF` is generated directly from their own `INIT_REQ` and does not wait for `OUT.INIT_CNF`. A calling DEMUX or an [`AOption_T_2`](../splitting/AOption_T_2.md) therefore sees the input as confirmed before the downstream group has finished initializing.
- **No latch:** the block stores nothing; the current `QI_OUT` values of the two sockets are authoritative.
- **A DEMUX output only once:** if the same output must also feed a group of its own, an [`AOption_T_2`](../splitting/AOption_T_2.md) is inserted first.

## Application Scenarios

- A shared option group that should be active for several options (`K = 2` or `K = 3`) but not for `K = 1` (`DummyGroup`).

## Summary

`AOption_OR_2` merges two `AOption` requests into one using an `OR_BOOL_2`: `OUT` is active when `IN1` or `IN2` is active.

## 🛠️ Related Exercises

- [Uebung_094b3e_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3e_AX/)
- [Uebung_094b3f_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3f_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
