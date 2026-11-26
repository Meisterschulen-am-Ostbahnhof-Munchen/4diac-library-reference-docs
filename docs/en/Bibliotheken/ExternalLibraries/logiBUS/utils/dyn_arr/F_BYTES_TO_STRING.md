# F_BYTES_TO_STRING

![F_BYTES_TO_STRING](./F_BYTES_TO_STRING.svg)

* * * * * * * * * *

## Introduction

The function `F_BYTES_TO_STRING` (version 1.1) reads the first `SIZE` bytes of a `BYTE` array as a `STRING`. It is the counterpart of [F_STRING_TO_BYTES_TERM](F_STRING_TO_BYTES_TERM.md) and [F_STRING_TO_BYTES](F_STRING_TO_BYTES.md). It belongs to the package `logiBUS::utils::dyn_arr` of the library `logiBUS_utils` 3.0.0.

## Interface structure

### **Event inputs**

-   **REQ** (Type: `Event`): Starts the read (with data: `SIZE`, `STOP_AT_NUL`, `DATA`)

### **Event outputs**

-   **CNF** (Type: `Event`): Read finished (with data: return value, `DATA`)

### **Data inputs**

-   **SIZE** (Type: `UINT`): Number of valid bytes (for example `u16DaSize`), at most the size of `DATA`
-   **STOP_AT_NUL** (Type: `BOOL`): TRUE: the text ends before the first `16#00` (default: `TRUE`)
-   **DATA** (Type: `ARRAY[*] OF BYTE`, IN/OUT): Source buffer, for example from `PROVIDE_ARR_xxxx_BYTE`

### **Data outputs**

-   **Return value** (Type: `STRING`): Text, 1 character per byte

## How it works

The array is read up to `SIZE`. With `STOP_AT_NUL = TRUE` the text ends before the first `16#00`. Data written with `F_STRING_TO_BYTES` (without `16#00`) should be read with a matching `SIZE`, because there is no text end.

Note: This page was generated from the type definition (`F_BYTES_TO_STRING.fct`, state 2026-11-26). Not tested on hardware.
