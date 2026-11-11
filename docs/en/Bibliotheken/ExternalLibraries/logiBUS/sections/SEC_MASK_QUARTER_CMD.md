# SEC_MASK_QUARTER_CMD

![SEC_MASK_QUARTER_CMD](./SEC_MASK_QUARTER_CMD.svg)

* * * * * * * * * *

## Introduction

**SEC_MASK_QUARTER_CMD** blocks sections in a condensed work state using a BIT mask (1 = section free). The block is meant for the setpoint (Setpoint Condensed Work State). It is a Simple FB with `REQ`/`CNF`, `IN` (`ARRAY[0..15] OF DWORD`, 2 bits per section), `MASK` (`ARRAY[0..7] OF DWORD`, 1 bit per section) and `OUT` (`ARRAY[0..15] OF DWORD`).

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

Blocked sections always become `00` (off), even if the input contains `10` or `11`. Free sections stay unchanged.

| Input (section blocked) | Output |
| ----------------------- | ------ |
| `00` | `00` |
| `01` | `00` |
| `10` | `00` |
| `11` | `00` |

Section s is in the condensed work state in word `(s-1) DIV 16`, bits `2*((s-1) MOD 16)+1..2*((s-1) MOD 16)`, in the BIT mask in word `(s-1) DIV 32`, bit `(s-1) MOD 32`.

## Technical Details

- For the setpoint, `11` means "no change". If a blocked section passed `11` through, it would stay on after an earlier `01`. It is therefore forced to `00`.
- There are two mask blocks because `11` means different things for the setpoint and the actual value. See [Condensed Work State](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/en/latest/Condensed-Work-State/) and [`SEC_MASK_QUARTER_ACT`](SEC_MASK_QUARTER_ACT.md).
- The mask typically comes from a BIT switcher, for example [`SEC_SWITCH_LR_BIT`](SEC_SWITCH_LR_BIT.md).
- The loop runs over all 256 sections (16 words).

## Conclusion

`SEC_MASK_QUARTER_CMD` applies a BIT mask to a setpoint condensed work state and forces blocked sections to `00` (off). Free sections stay unchanged.
