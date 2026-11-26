# F_STRING_TO_BYTES

![F_STRING_TO_BYTES](./F_STRING_TO_BYTES.svg)

* * * * * * * * * *

## Introduction

The function `F_STRING_TO_BYTES` (version 2.0) copies the characters of a `STRING` into a `BYTE` array. Only the characters are copied, **without** the text terminator `16#00`. This suits files, for example with `FsWrite` (exercise 156). If the text should end with `16#00`, e.g. for strings over CAN, use [F_STRING_TO_BYTES_TERM](F_STRING_TO_BYTES_TERM.md). It belongs to the package `logiBUS::utils::dyn_arr` of the library `logiBUS_utils` 3.0.0.

## Interface structure

### **Event inputs**

-   **REQ** (Type: `Event`): Starts the copy (with data: `IN`, `DATA`)

### **Event outputs**

-   **CNF** (Type: `Event`): Copy finished (with data: return value, `DATA`)

### **Data inputs**

-   **IN** (Type: `STRING`): Text, 1 byte per character
-   **DATA** (Type: `ARRAY[*] OF BYTE`, IN/OUT): Target buffer, for example from `PROVIDE_ARR_xxxx_BYTE`. The whole buffer is written.

### **Data outputs**

-   **Return value** (Type: `UINT`): Number of characters, at most the size of `DATA`

## How it works

1. The length of `IN` is limited to the size of `DATA`.
2. All bytes of `DATA` are filled with `16#FF` (unused bytes, as usual for J1939/ISOBUS).
3. The characters of `IN` are copied in from the first index of `DATA`.
4. The number of copied characters is returned.

## Change compared with version 1.1

Up to version 1.1 a `16#00` followed the characters (if there was room). Version 2.0 no longer writes a `16#00`; the return value is the plain character count. The old behavior is now offered by `F_STRING_TO_BYTES_TERM`.

**Compatibility:** Old programs that use `F_STRING_TO_BYTES` for strings over CAN no longer send a `16#00` with the new ELF. The receiver then finds no text end with `F_BYTES_TO_STRING`. Such programs must be switched to `F_STRING_TO_BYTES_TERM`. Exercises 138, 138b, 142, 144, 147, 149 and 149d already use `F_STRING_TO_BYTES_TERM`.

Note: This page was generated from the type definition (`F_STRING_TO_BYTES.fct`, state 2026-11-26). Not tested on hardware.
