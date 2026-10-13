# AOption_AD_DEMUX_7

![AOption_AD_DEMUX_7](AOption_AD_DEMUX_7.svg)

* * * * * * * * * *

## Introduction

The function block AOption_AD_DEMUX_7 is a demultiplexer (native generic FB, no ECC and no algorithm in this type) for 7 option outputs of type `Option::AOption`. Selection is done by a **bit mask** on the bidirectional adapter `K` (`adapter::types::bidirectional::AD2`, DWORD). This is the multi-select variant: any number of outputs can be active at the same time.

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

- **K** (Socket, `adapter::types::bidirectional::AD2`): bit mask (bit 0 = OUT1, bit 1 = OUT2, ...). Bidirectional: `EO1`/`DO1` = new bit mask request comes in, `EI1`/`DI1` = acknowledgement with the accepted bit mask goes back
- **OUT1** (Plug, `Option::AOption`): option 1, active when bit 0 of K is set (value 1)
- **OUT2** (Plug, `Option::AOption`): option 2, active when bit 1 of K is set (value 2)
- **OUT3** (Plug, `Option::AOption`): option 3, active when bit 2 of K is set (value 4)
- **OUT4** (Plug, `Option::AOption`): option 4, active when bit 3 of K is set (value 8)
- **OUT5** (Plug, `Option::AOption`): option 5, active when bit 4 of K is set (value 16)
- **OUT6** (Plug, `Option::AOption`): option 6, active when bit 5 of K is set (value 32)
- **OUT7** (Plug, `Option::AOption`): option 7, active when bit 6 of K is set (value 64)

Each output `OUTx` uses the adapter `Option::AOption`: the event `INIT_REQ` (together with `QI_OUT`) runs in the plug → socket direction, the confirmation `INIT_CNF` in the opposite direction.

## Functionality

**Bit assignment (multi-select):**

- Bit 0 (value 1) — `OUT1`
- Bit 1 (value 2) — `OUT2`
- Bit 2 (value 4) — `OUT3`
- Bit 3 (value 8) — `OUT4`
- Bit 4 (value 16) — `OUT5`
- Bit 5 (value 32) — `OUT6`
- Bit 6 (value 64) — `OUT7`

Example: `K = 3` (bit 0 and bit 1 set) activates `OUT1` and `OUT2`, all other outputs are off. Any number of outputs, or all of them, can be active simultaneously.

**Switching behavior:** Only bits that change compared to the previous `K` value trigger an `INIT_REQ` on the corresponding output (0→1: `QI=TRUE`, activate; 1→0: `QI=FALSE`, deactivate). Unchanged bits are not triggered again — each output is triggered at most once per incoming `K` value.

**Busy gating via the bidirectional K adapter:** As long as `INIT_CNF` is still awaited from at least one output affected by the last K change, every further bit mask request arriving via `K` (plug → socket direction, `EO1`/`DO1`) is ignored — it is neither buffered nor replayed. Once all triggered `INIT_CNF` have arrived, the block reports this back via `K` (socket → plug direction, `EI1`/`DI1`) with the accepted bit mask (not with the state of the outputs: bits beyond the available outputs are acknowledged unchanged as well). Only then are new K requests accepted again.

## ⚖️ Comparison with Similar Blocks

[AOption_AUI_DEMUX_7](AOption_AUI_DEMUX_7.md) is the single-select variant with an index (`AUI2`) instead of a bit mask: at most one output is active there.
