# ASR2_SPLIT_FWD_REV

![ASR2_SPLIT_FWD_REV](ASR2_SPLIT_FWD_REV.svg)

![ASR2_SPLIT_FWD_REV_network](./ASR2_SPLIT_FWD_REV_network.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`ASR2_SPLIT_FWD_REV` passes the bidirectional adapter `ASR2` through from `IN` to `OUT` (passthrough) and additionally mirrors the **forward channel** to `FWD_OUT` and the **reverse channel** to `REV_OUT`. Both mirrors are unidirectional `ASR` adapters.

## Interface structure

### **Event inputs**

No direct events.

### **Event outputs**

No direct events.

### **Data inputs**

No direct data.

### **Data outputs**

No direct data.

### **Adapters**

| Role | Name | Type | Description |
| --- | --- | --- | --- |
| Socket | IN | adapter::types::bidirectional::ASR2 | bidirektionaler Eingang |
| Plug | OUT | adapter::types::bidirectional::ASR2 | bidirektionaler Ausgang (Passthrough) |
| Plug | FWD_OUT | adapter::types::unidirectional::ASR | spiegelt den Vorwaerts-Kanal (IN.EO*) |
| Plug | REV_OUT | adapter::types::unidirectional::ASR | spiegelt den Rueckwaerts-Kanal (OUT.EI*) |

## How it works

All connections are direct event and data connections, there is no logic of its own. The list is in the section “Connections inside the block”.

## Connections inside the block

**Event connections:**

- `IN.EO_SET` → `OUT.EO_SET`
- `IN.EO_SET` → `FWD_OUT.SET`
- `OUT.EI_SET` → `IN.EI_SET`
- `OUT.EI_SET` → `REV_OUT.SET`
- `IN.EO_RESET` → `OUT.EO_RESET`
- `IN.EO_RESET` → `FWD_OUT.RESET`
- `OUT.EI_RESET` → `IN.EI_RESET`
- `OUT.EI_RESET` → `REV_OUT.RESET`

## Technical notes

- Pure wiring block (composite without internal function blocks).
- The mirror is used to tap a channel without disturbing the passthrough.

## ⚖️ Comparison with similar blocks

- Same family: [AB2_SPLIT_FWD_REV](../AB2_SPLIT_FWD_REV.md) and others in the parent folder.
