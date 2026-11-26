# F_STRING_TO_BYTES_TERM

![F_STRING_TO_BYTES_TERM](./F_STRING_TO_BYTES_TERM.svg)

* * * * * * * * * *

## Introduction

The function `F_STRING_TO_BYTES_TERM` (version 1.0) copies the characters of a `STRING` into a `BYTE` array and ends the text with `16#00` if there is room. It behaves like `F_STRING_TO_BYTES` up to version 1.1. It is meant for strings over CAN (`AlPgnTxNew_TP`, `DlPgnSend`), so that `F_BYTES_TO_STRING` finds the text end at the receiver. For files without a terminator use [F_STRING_TO_BYTES](F_STRING_TO_BYTES.md). It belongs to the package `logiBUS::utils::dyn_arr` of the library `logiBUS_utils` 3.0.0.

## Interface structure

### **Event inputs**

-   **REQ** (Type: `Event`): Starts the copy (with data: `IN`, `DATA`)

### **Event outputs**

-   **CNF** (Type: `Event`): Copy finished (with data: return value, `DATA`)

### **Data inputs**

-   **IN** (Type: `STRING`): Text, 1 byte per character
-   **DATA** (Type: `ARRAY[*] OF BYTE`, IN/OUT): Target buffer, for example from `PROVIDE_ARR_xxxx_BYTE`. The whole buffer is written.

### **Data outputs**

-   **Return value** (Type: `UINT`): Characters + 1 (the `16#00`), at most the size of `DATA`

## How it works

1. The characters of `IN` are copied into `DATA`.
2. `16#00` follows as the text end, if there is room.
3. All remaining bytes are `16#FF`.

The counterpart is [F_BYTES_TO_STRING](F_BYTES_TO_STRING.md): it reads `SIZE` bytes and stops at the first `16#00`.

Note: This page was generated from the type definition (`F_STRING_TO_BYTES_TERM.fct`, state 2026-11-26). Not tested on hardware.
