# AOption_AUI_DEMUX_7

![AOption_AUI_DEMUX_7](AOption_AUI_DEMUX_7.svg)

* * * * * * * * * *

## Introduction

The function block AOption_AUI_DEMUX_7 is a demultiplexer (native generic FB, no ECC and no algorithm in this type) for 7 option outputs of type `Option::AOption`. Selection is done by an **index** on the bidirectional adapter `K` (`adapter::types::bidirectional::AUI2`, UINT). This is the single-select variant: at most one output is active at a time.

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
- **OUT2** (Plug, `Option::AOption`): option 2, selected when K = 2
- **OUT3** (Plug, `Option::AOption`): option 3, selected when K = 3
- **OUT4** (Plug, `Option::AOption`): option 4, selected when K = 4
- **OUT5** (Plug, `Option::AOption`): option 5, selected when K = 5
- **OUT6** (Plug, `Option::AOption`): option 6, selected when K = 6
- **OUT7** (Plug, `Option::AOption`): option 7, selected when K = 7

Each output `OUTx` uses the adapter `Option::AOption`: the event `INIT_REQ` (together with `QI_OUT`) runs in the plug → socket direction, the confirmation `INIT_CNF` in the opposite direction.

## Functionality

**Index assignment:**

- `K = 0` — all outputs OFF
- `K = 1` — `OUT1` ON, all others OFF
- `K = 2` — `OUT2` ON, all others OFF
- `K = 3` — `OUT3` ON, all others OFF
- `K = 4` — `OUT4` ON, all others OFF
- `K = 5` — `OUT5` ON, all others OFF
- `K = 6` — `OUT6` ON, all others OFF
- `K = 7` — `OUT7` ON, all others OFF
- any other value (> 7) — all outputs OFF

**Switching behavior (break-before-make):** If `K` changes from one active output to another, the previously active output is deactivated first (`INIT_REQ` with `QI=FALSE`). Only once `INIT_CNF` has arrived from that output is the newly selected output activated (`INIT_REQ` with `QI=TRUE`). Thus never more than one option is active at the same time. Each output is triggered at most once per incoming `K` value; unaffected outputs are left untouched.

**Busy gating via the bidirectional K adapter:** While a switchover is in progress (waiting for `INIT_CNF` from the previous and/or new output), every further index request arriving via `K` (plug → socket direction, `EO1`/`DO1`) is ignored — it is neither buffered nor replayed. When finished, the block reports this back via `K` (socket → plug direction, `EI1`/`DI1`) with the accepted K value (not with the state of the outputs: an invalid value is acknowledged unchanged as well). Only then are new index requests accepted again. What counts is always the state after completion of the last change actually processed.

## ⚖️ Comparison with Similar Blocks

[AOption_AD_DEMUX_7](AOption_AD_DEMUX_7.md) is the multi-select variant with a bit mask (`AD2`): several outputs can be active at the same time there.
