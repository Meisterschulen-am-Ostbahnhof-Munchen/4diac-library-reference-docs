# SPLIT_LWORD_INTO_BYTES

![SPLIT_LWORD_INTO_BYTES](SPLIT_LWORD_INTO_BYTES.svg)

* * * * * * * * * *

## Introduction

The function `SPLIT_LWORD_INTO_BYTES` splits a 64-bit long word (`LWORD`) into its eight bytes (`BYTE`).

## Interface Structure

### **Event Inputs**

- `REQ`: Starts the processing. Associated data: `IN`

### **Event Outputs**

- `CNF`: Signals the processing. Associated data: `BYTE_00` to `BYTE_07`

### **Data Inputs**

- `IN`: The long word to be split (`LWORD`)

### **Data Outputs**

- `BYTE_00`: Byte 0 (`BYTE`), least significant byte (bits 0 to 7)
- `BYTE_01`: Byte 1 (`BYTE`), bits 8 to 15
- `BYTE_02`: Byte 2 (`BYTE`), bits 16 to 23
- `BYTE_03`: Byte 3 (`BYTE`), bits 24 to 31
- `BYTE_04`: Byte 4 (`BYTE`), bits 32 to 39
- `BYTE_05`: Byte 5 (`BYTE`), bits 40 to 47
- `BYTE_06`: Byte 6 (`BYTE`), bits 48 to 55
- `BYTE_07`: Byte 7 (`BYTE`), most significant byte (bits 56 to 63)

### **Adapters**

This function does not use any adapters.

## How it works

The function reads the eight bytes from the input `IN` via the partial access `.%B0` to `.%B7`:

```text
BYTE_00 := IN.%B0;
BYTE_01 := IN.%B1;
BYTE_02 := IN.%B2;
BYTE_03 := IN.%B3;
BYTE_04 := IN.%B4;
BYTE_05 := IN.%B5;
BYTE_06 := IN.%B6;
BYTE_07 := IN.%B7;
```

`BYTE_00` is thus the least significant and `BYTE_07` the most significant byte of the `LWORD`.

## Technical Details

- Implemented in Structured Text (ST) as a `FUNCTION` without internal state
- Part of the package `eclipse4diac::utils::splitting`
- Version 1.0 (2026-05-16)

## Application Scenarios

- Splitting a 64-bit value into individual bytes, e.g. for telegrams or bus messages (CAN: 8 data bytes)
- Byte-wise analysis of 64-bit bit patterns

## Summary

`SPLIT_LWORD_INTO_BYTES` extracts the eight bytes of an `LWORD`, starting with the least significant one.
