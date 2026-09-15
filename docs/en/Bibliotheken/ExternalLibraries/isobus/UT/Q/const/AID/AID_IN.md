# AID_IN

![AID_IN](./AID_IN.svg)

* * * * * * * * * *

## Introduction

AID_IN is a global constants container used in 4diac-ide for ISO 11783 / ISOBUS applications. It defines the numeric attribute IDs for an Input Number object on a Virtual Terminal. The provided XML declares named constants of type `USINT` with values from `1` to `15`, allowing the attribute IDs to be referenced by meaningful names instead of magic numbers.

## Interface Structure

The AID_IN element does not define an IEC 61499 function block interface. It contains no event inputs, event outputs, data inputs, data outputs, or adapters.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

AID_IN provides the attribute identifiers for an ISOBUS Input Number object. In accordance with ISO 11783-6 Table B.18, attribute IDs enclosed in square brackets `[ ]` (such as `[14]` `VALUE` and `[15]` `OPTIONS_2`) indicate **read-only attributes** for the *Change Attribute* command, accessible via the *Get Attribute Value* message (F.58):

| Constant | Value | Description |
|---|---|---|
| `WIDTH` | `USINT#1` | Width in pixels. (Writable via *Change Attribute* F.38) |
| `HEIGHT` | `USINT#2` | Height in pixels. (Writable via *Change Attribute* F.38) |
| `BACKGROUND_COLOUR` | `USINT#3` | Background colour index. (Writable via *Change Attribute* F.38) |
| `FONT_ATT` | `USINT#4` | Object ID of a Font Attributes object. (Writable via *Change Attribute* F.38) |
| `OPTIONS` | `USINT#5` | Bitmask: Bit 0 = Transparent, Bit 1 = Display leading zeros, Bit 2 = Display zero as blank, Bit 3 = Truncate decimals. (Writable via *Change Attribute* F.38) |
| `VARIABLE_REF` | `USINT#6` | Object ID of a Variable object. (Writable via *Change Attribute* F.38) |
| `MIN_VALUE` | `USINT#7` | Minimum value. (Writable via *Change Attribute* F.38) |
| `MAX_VALUE` | `USINT#8` | Maximum value. (Writable via *Change Attribute* F.38) |
| `OFFSET` | `USINT#9` | Offset. (Writable via *Change Attribute* F.38) |
| `SCALE` | `USINT#10` | Scale. (Writable via *Change Attribute* F.38) |
| `NUMB_DECIMALS` | `USINT#11` | Number of decimals. (Writable via *Change Attribute* F.38) |
| `FORMAT` | `USINT#12` | Decimal display format. `0` means fixed format decimal, `1` means exponential format. (Writable via *Change Attribute* F.38) |
| `JUSTIFICATION` | `USINT#13` | Justification. Bits 0-1 define horizontal alignment, bits 2-3 define vertical alignment. (Writable via *Change Attribute* F.38) |
| `VALUE` | `USINT#[14]` | **Read-only** for *Change Attribute* (ISO 11783-6). Raw unsigned value of input field before scaling. Queryable via *Get Attribute Value* (F.58); updated at runtime via *Change Numeric Value* command (F.22). |
| `OPTIONS_2` | `USINT#[15]` | **Read-only** for *Change Attribute* (ISO 11783-6). Bitmask: Bit 0 = Enabled, Bit 1 = Real-time editing. Queryable via *Get Attribute Value* (F.58); managed via *Select Input Object* (F.6). |

## Application Scenarios

AID_IN is useful in ISOBUS applications that need to configure or query Input Number objects on a Virtual Terminal. Typical scenarios include:

- Creating attribute lists for ISOBUS object pool configuration via *Change Attribute* (F.38).
- Querying the current raw value (`AID_IN.VALUE` `[14]`) via *Get Attribute Value* (F.58) or updating it at runtime via *Change Numeric Value* (F.22).
- Managing focus or operational status (`AID_IN.OPTIONS_2` `[15]`) via *Select Input Object* (F.6).

## Conclusion

AID_IN provides a clear and maintainable way to handle ISOBUS Input Number object attribute IDs in 4diac-ide.
