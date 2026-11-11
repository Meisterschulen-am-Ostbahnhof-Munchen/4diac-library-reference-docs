# SEC_MASK_QUARTER_ACT

![SEC_MASK_QUARTER_ACT](./SEC_MASK_QUARTER_ACT.svg)

* * * * * * * * * *

## Introduction

**SEC_MASK_QUARTER_ACT** blocks sections in a condensed work state using a BIT mask (1 = section free). The block is meant for the actual value (Actual Condensed Work State). It is a Simple FB with `REQ`/`CNF`, `IN` (`ARRAY[0..15] OF DWORD`, 2 bits per section), `MASK` (`ARRAY[0..7] OF DWORD`, 1 bit per section) and `OUT` (`ARRAY[0..15] OF DWORD`).

## Interface Structure

### **Event Inputs**

| Name | Type | Comment | Associated data |
| ---- | ---- | ------- | --------------- |
| REQ | Event | Apply mask | IN, MASK |

### **Event Outputs**

| Name | Type | Comment | Associated data |
| ---- | ---- | ------- | --------------- |
| CNF | Event | Result calculated | OUT |

### **Data Inputs**

| Name | Type | Comment |
| ---- | ---- | ------- |
| IN | DWORD[16] | Condensed work state, 2 bits per section, word k = sections 16k+1..16k+16 |
| MASK | DWORD[8] | BIT mask, 1 = section free, word k = sections 32k+1..32k+32 |

### **Data Outputs**

| Name | Type | Comment |
| ---- | ---- | ------- |
| OUT | DWORD[16] | Result after applying the mask, same layout as `IN` |

### **Adapters**

None.

## Functionality

For blocked sections only `01` (on) becomes `00` (off). `00`, `10` (error) and `11` (not installed) stay visible. Free sections stay unchanged.

| Input (section blocked) | Output |
| ----------------------- | ------ |
| `00` | `00` |
| `01` | `00` |
| `10` | `10` |
| `11` | `11` |

Section s is in the condensed work state in word `(s-1) DIV 16`, bits `2*((s-1) MOD 16)+1..2*((s-1) MOD 16)`, in the BIT mask in word `(s-1) DIV 32`, bit `(s-1) MOD 32`.

## Technical Details

- For the actual value, `11` means "not installed" and `10` "error". This information should be preserved for blocked sections too, only the state "on" is hidden.
- There are two mask blocks because `11` means different things for the setpoint and the actual value. See [Condensed Work State](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/en/latest/Condensed-Work-State/) and [`SEC_MASK_QUARTER_CMD`](SEC_MASK_QUARTER_CMD.md).
- The mask typically comes from a BIT switcher, for example [`SEC_SWITCH_LR_BIT`](SEC_SWITCH_LR_BIT.md).
- The loop runs over all 256 sections (16 words).

## Conclusion

`SEC_MASK_QUARTER_ACT` applies a BIT mask to a actual-value condensed work state and preserves the meaning of the 2-bit values.
