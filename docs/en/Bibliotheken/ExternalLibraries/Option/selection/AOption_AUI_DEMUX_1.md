# AOption_AUI_DEMUX_1

![AOption_AUI_DEMUX_1](AOption_AUI_DEMUX_1.svg)

* * * * * * * * * *

## Introduction

The function block AOption_AUI_DEMUX_1 is a demultiplexer (native generic FB, no ECC and no algorithm in this type) for 1 option output of type `Option::AOption`. Selection is done by an **index** on the bidirectional adapter `K` (`adapter::types::bidirectional::AUI2`, UINT). This is the single-select variant: at most one output is active at a time.

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs — communication happens exclusively through the adapters.

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

The function block has no direct data inputs.

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **K** (Socket, `adapter::types::bidirectional::AUI2`): index. Bidirectional: `EO1`/`DO1` = new K request comes in, `EI1`/`DI1` = acknowledgement with the accepted value goes back
- **OUT1** (Plug, `Option::AOption`): option 1, selected when K = 1

Each output `OUTx` uses the adapter `Option::AOption`: the event `INIT_REQ` (together with `QI_OUT`) runs in the plug → socket direction, the confirmation `INIT_CNF` in the opposite direction.

## Functionality

**Index assignment:**

- `K = 0` — all outputs OFF
- `K = 1` — `OUT1` ON, all others OFF
- any other value (> 1) — all outputs OFF

**Switching behavior (break-before-make):** If `K` changes from one active output to another, the previously active output is deactivated first (`INIT_REQ` with `QI=FALSE`). Only once `INIT_CNF` has arrived from that output is the newly selected output activated (`INIT_REQ` with `QI=TRUE`). Thus never more than one option is active at the same time. Each output is triggered at most once per incoming `K` value; unaffected outputs are left untouched.

**Busy gating via the bidirectional K adapter:** While a switchover is in progress (waiting for `INIT_CNF` from the previous and/or new output), every further index request arriving via `K` (plug → socket direction, `EO1`/`DO1`) is ignored — it is neither buffered nor replayed. When finished, the block reports this back via `K` (socket → plug direction, `EI1`/`DI1`) with the accepted K value (not with the state of the outputs: an invalid value is acknowledged unchanged as well). Only then are new index requests accepted again. What counts is always the state after completion of the last change actually processed.

## ⚖️ Comparison with Similar Blocks

[AOption_AD_DEMUX_1](AOption_AD_DEMUX_1.md) is the multi-select variant with a bit mask (`AD2`): several outputs can be active at the same time there.

## 🛠️ Related Exercises

- [Uebung_094c2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094c2_AX/)
- [Uebung_094c2b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094c2b_AX/)
- [Uebung_094c_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094c_AX/)

