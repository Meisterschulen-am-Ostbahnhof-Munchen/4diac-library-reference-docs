# AIS_TO_AIWS

![AIS_TO_AIWS](AIS_TO_AIWS.svg)

![AIS_TO_AIWS_network](./AIS_TO_AIWS_network.svg)

* * * * * * * * * *

!!! warning "Not released yet"
    The block is defined as a type in the library (as of 2026-11-13). The matching firmware side (C++ and ELF libraries) has not been released yet. Do not assume it is runnable yet.

## Introduction

`AIS_TO_AIWS` is a composite block: Composite FB for converting AIS (STRING) adapter to AIWS (WSTRING) adapter.

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
| Socket | AIS_IN | adapter::types::unidirectional::AIS | STRING Adapter Input |
| Plug | AIWS_OUT | adapter::types::unidirectional::AIWS | WSTRING Adapter Output |

## How it works

The connections are listed in the section “Connections inside the block”. Internally `iec61131::conversion::F_STRING_TO_WSTRING` is used.

## Connections inside the block

**Internal blocks:** `Convert` (`iec61131::conversion::F_STRING_TO_WSTRING`)

**Event connections:**

- `AIS_IN.E1` → `Convert.REQ`
- `Convert.CNF` → `AIWS_OUT.E1`

**Data connections:**

- `AIS_IN.D1` → `Convert.IN`
- `Convert.OUT` → `AIWS_OUT.D1`

## Technical notes

- Initial Version - Luecke in der Adapter-Familie geschlossen

## ⚖️ Comparison with similar blocks

Blocks of the same family are in the same folder or in the neighbouring folders of `conversion`.
