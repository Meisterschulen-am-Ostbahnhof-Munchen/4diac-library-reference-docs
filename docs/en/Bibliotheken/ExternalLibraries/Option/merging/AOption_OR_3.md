# AOption_OR_3

![AOption_OR_3](./AOption_OR_3.svg)

![AOption_OR_3_network](./AOption_OR_3_network.svg)

* * * * * * * * * *

## Introduction

`AOption_OR_3` is a composite FB that serves as a **fan-in** (OR combination) for the option adapter [`AOption`](../AOption.md): three option requests at the sockets `IN1`, `IN2` and `IN3` are merged into a single request at the plug `OUT`. `OUT` is active as long as `IN1` **or** `IN2` **or** `IN3` is active. Variant of [`AOption_OR_2`](AOption_OR_2.md) with 3 inputs.

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
| `IN3` | `Option::AOption` | Socket | Third AOption input. |
| `OUT` | `Option::AOption` | Plug | Common output - active as long as `IN1` OR `IN2` is active. |

## Functionality

The internal network contains three FBs: `E_MUX_3` (`iec61499::events::E_MUX_3`), `OR_QI` (`iec61131::booleanOperators::OR_BOOL_3`) and `E_DEMUX_3` (`iec61499::events::E_DEMUX_3`).

Events:

- `IN1.INIT_REQ` -> `E_MUX_3.EI1` (`K = 0`), `IN2.INIT_REQ` -> `E_MUX_3.EI2` (`K = 1`) and `IN3.INIT_REQ` -> `E_MUX_3.EI3` (`K = 2`)
- `E_MUX_3.EO` -> `OR_QI.REQ`
- `OR_QI.CNF` -> `OUT.INIT_REQ`
- `OUT.INIT_CNF` -> `E_DEMUX_3.EI`
- `E_DEMUX_3.EO1` -> `IN1.INIT_CNF`, `E_DEMUX_3.EO2` -> `IN2.INIT_CNF` and `E_DEMUX_3.EO3` -> `IN3.INIT_CNF`

Data:

- `IN1.QI_OUT` -> `OR_QI.IN1`, `IN2.QI_OUT` -> `OR_QI.IN2`, `IN3.QI_OUT` -> `OR_QI.IN3`
- `OR_QI.OUT` -> `OUT.QI_OUT`
- `E_MUX_3.K` -> `E_DEMUX_3.K`

`E_MUX_3` remembers in `K` which socket the request came from, and only then triggers the OR operation. Its result is passed to `OUT` as `INIT_REQ` with `QI_OUT`. `E_DEMUX_3` routes the confirmation `OUT.INIT_CNF` back, based on `K`, to exactly the socket that made the request.

## Technical Notes

- **Confirmation only after OUT:** an input is confirmed only once the group at `OUT` has finished. An upstream DEMUX or [`AOption_T_2`](../splitting/AOption_T_2.md) therefore sees the chain as done only when it actually is.
- **Order enforced by wiring:** because `OR_QI` is triggered via `E_MUX_3.EO`, `K` is always set before `OUT.INIT_REQ` is sent.
- **Only one outstanding request:** `K` holds exactly one sender. If a second request arrives before `OUT.INIT_CNF` has returned, the assignment is lost. Behind a DEMUX (break-before-make) this cannot happen; independent sources would need a basic FB with an ECC.
- **A DEMUX output only once:** if the same output must also feed a group of its own, an [`AOption_T_2`](../splitting/AOption_T_2.md) is inserted first.

## Application Scenarios

- A shared option group that should be active for several options without cascading `AOption_OR_2`.

## Summary

`AOption_OR_3` merges three `AOption` requests into one (`OUT` active when `IN1`, `IN2` or `IN3` is active) and returns the confirmation via `E_MUX_3`/`E_DEMUX_3` to the requesting socket.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
