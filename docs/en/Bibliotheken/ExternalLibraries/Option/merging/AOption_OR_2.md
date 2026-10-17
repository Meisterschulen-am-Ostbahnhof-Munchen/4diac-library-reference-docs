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

The internal network contains three FBs: `E_MUX_2` (`iec61499::events::E_MUX_2`), `OR_QI` (`iec61131::booleanOperators::OR_BOOL_2`) and `E_DEMUX_2` (`iec61499::events::E_DEMUX_2`).

Events:

- `IN1.INIT_REQ` -> `E_MUX_2.EI1` (`K = 0`) and `IN2.INIT_REQ` -> `E_MUX_2.EI2` (`K = 1`)
- `E_MUX_2.EO` -> `OR_QI.REQ`
- `OR_QI.CNF` -> `OUT.INIT_REQ`
- `OUT.INIT_CNF` -> `E_DEMUX_2.EI`
- `E_DEMUX_2.EO1` -> `IN1.INIT_CNF` and `E_DEMUX_2.EO2` -> `IN2.INIT_CNF`

Data:

- `IN1.QI_OUT` -> `OR_QI.IN1`, `IN2.QI_OUT` -> `OR_QI.IN2`
- `OR_QI.OUT` -> `OUT.QI_OUT`
- `E_MUX_2.K` -> `E_DEMUX_2.K`

`E_MUX_2` remembers in `K` which socket the request came from, and only then triggers the OR operation. Its result is passed to `OUT` as `INIT_REQ` with `QI_OUT`. `E_DEMUX_2` routes the confirmation `OUT.INIT_CNF` back, based on `K`, to exactly the socket that made the request.

## Technical Notes

- **Confirmation only after OUT:** an input is confirmed only once the group at `OUT` has finished. An upstream DEMUX or [`AOption_T_2`](../splitting/AOption_T_2.md) therefore sees the chain as done only when it actually is.
- **Order enforced by wiring:** because `OR_QI` is triggered via `E_MUX_2.EO`, `K` is always set before `OUT.INIT_REQ` is sent.
- **Only one outstanding request:** `K` holds exactly one sender. If a second request arrives before `OUT.INIT_CNF` has returned, the assignment is lost. Behind a DEMUX (break-before-make) this cannot happen; independent sources would need a basic FB with an ECC.
- **A DEMUX output only once:** if the same output must also feed a group of its own, an [`AOption_T_2`](../splitting/AOption_T_2.md) is inserted first.

## Application Scenarios

- A shared option group that should be active for several options (`K = 2` or `K = 3`) but not for `K = 1` (`DummyGroup`).

## Summary

`AOption_OR_2` merges two `AOption` requests into one (`OUT` active when `IN1` or `IN2` is active) and returns the confirmation via `E_MUX_2`/`E_DEMUX_2` to the requesting socket.

## 🛠️ Related Exercises

- [Uebung_094b3e_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3e_AX/)
- [Uebung_094b3f_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3f_AX/)

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
