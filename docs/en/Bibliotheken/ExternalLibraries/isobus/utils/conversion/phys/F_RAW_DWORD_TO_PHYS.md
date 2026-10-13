# F_RAW_DWORD_TO_PHYS

![F_RAW_DWORD_TO_PHYS](F_RAW_DWORD_TO_PHYS.svg)

* * * * * * * * * *

## Introduction

The function `F_RAW_DWORD_TO_PHYS` converts an ISOBUS raw value (`DWORD`) into a physical `REAL` value. The ISOBUS formula is: `display = (raw + offset) * scale`.

## Interface Structure

### **Event Inputs**

- **REQ**: starts the conversion. Associated data: `dwRaw`, `stObj`.

### **Event Outputs**

- **CNF**: signals the completed conversion. The result is available at the data output.

### **Data Inputs**

- **dwRaw** (`DWORD`): ISOBUS raw value
- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): object pool properties of the number variable (offset `i32Offset`, scale `r32Scale`)

### **Data Outputs**

- **(unnamed)** (`REAL`): physical value (return value of the function)

### **Adapters**

No adapters available.

## Functionality

1. `dwRaw` is interpreted as an unsigned number (`DWORD_TO_UDINT`) and widened to `LINT` (`i64Raw`).
2. `stObj.i32Offset` is used as `LINT` without conversion (`i64Offset`).
3. The result is `LINT_TO_REAL(i64Raw + i64Offset) * stObj.r32Scale`.

**Example:** `raw = 50000`, `scale = 0.01`, `offset = -100000` → `50000 + (-100000) = -50000` → `-50000 * 0.01 = -500.0`.

## Technical Details

- Implemented as an ST function (`FUNCTION ... : REAL`) in package `logiBUS::utils::conversion::phys`.
- The intermediate values are `LINT` to avoid overflow.
- The raw value is treated as an unsigned bit pattern and then widened to 64 bits.

## Application Scenarios

- Converting ISOBUS raw values (e.g. from a VT number variable) into physical quantities

## ⚖️ Comparison with Similar Building Blocks

- [F_RAW_TO_PHYS](./F_RAW_TO_PHYS.md) and [F_RAW_TO_PHYS_LREAL](./F_RAW_TO_PHYS_LREAL.md) are related conversions; the opposite direction is provided by [F_PHYS_TO_RAW](./F_PHYS_TO_RAW.md) and [F_PHYS_LREAL_TO_RAW](./F_PHYS_LREAL_TO_RAW.md).

## Conclusion

`F_RAW_DWORD_TO_PHYS` converts an ISOBUS `DWORD` raw value into a `REAL` value using offset and scale from the object pool.
