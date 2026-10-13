# AOption_AD_DEMUX_2

![AOption_AD_DEMUX_2](./AOption_AD_DEMUX_2.svg)

* * * * * * * * * *

## Introduction

`AOption_AD_DEMUX_2` is a bitmask demultiplexer (native generic FB `GEN_AOption_AD_DEMUX`, no ECC and no algorithm in this type) for `N=2` option outputs of type [`Option::AOption`](../AOption.md). Via the bidirectional bitmask adapter `K` (`adapter::types::bidirectional::AD2`, DWORD) any number of the 2 outputs can be activated at the same time (multi-select).

## Interface Structure

### **Event Inputs / Outputs**

No direct event connections; the events run through the adapters.

### **Data Inputs / Outputs**

No direct data connections; the data runs through the adapters.

### **Adapters**

| Adapter | Type | Direction | Comment |
|---------|------|-----------|---------|
| `OUT1` | `Option::AOption` | Plug | Option 1, active when bit 0 of `K` is set (value 1). |
| `OUT2` | `Option::AOption` | Plug | Option 2, active when bit 1 of `K` is set (value 2). |
| `K` | `adapter::types::bidirectional::AD2` | Socket | Bitmask (Bit0=OUT1, Bit1=OUT2, ...), bidirectional: `EO1`/`DO1` = new bitmask request in, `EI1`/`DI1` = acknowledgement with the accepted bitmask back. |

## Functionality

**Bit assignment (multi-select):**

- `Bit 0` (value `1`): `OUT1`
- `Bit 1` (value `2`): `OUT2`

Example: `K = 3` (bit 0 and bit 1 set) activates `OUT1` and `OUT2`; all other outputs stay off. Any number of outputs, or all of them, can be active at the same time.

**Switching behaviour:** Only bits that change compared to the previous `K` value trigger an `INIT_REQ` at the corresponding output (0->1: `QI=TRUE`, activate; 1->0: `QI=FALSE`, deactivate). Unchanged bits are not triggered again; each output is triggered at most once per incoming `K` value.

**Busy gating via the K adapter:** While `INIT_CNF` is still awaited from at least one output affected by the last `K` change, every further bitmask request arriving via `K` (plug -> socket, `EO1`/`DO1`) is ignored; it is neither buffered nor caught up later. Once all triggered `INIT_CNF` have arrived, the block reports this backwards via `K` (socket -> plug, `EI1`/`DI1`) with the accepted bitmask (not with the state of the outputs; bits beyond the existing outputs are acknowledged unchanged as well). Only then are new `K` requests accepted again.

## Technical Features

- **Multi-select**, in contrast to the single-select of `AOption_AUI_DEMUX_2`.
- **Native generic FB**: the behaviour is implemented in the runtime, not as ECC or algorithm.
- **Acknowledgement**: `K` is only reported back after all `INIT_CNF` have arrived; requests in between are lost.

## Application Scenarios

- Switching several optional function groups (e.g. [`OptionalGroup_001`](../../MyLib_AX_Option/sys/OptionalGroup_001.md)) on and off simultaneously via a bitmask.

## Conclusion

`AOption_AD_DEMUX_2` switches arbitrary combinations of 2 options via the bitmask `K` and reports the accepted bitmask only after all activations have completed.

## 🛠️ Related Exercises

- [Uebung_094b5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b5_AX/)
- [Uebung_094b5b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b5b_AX/)

