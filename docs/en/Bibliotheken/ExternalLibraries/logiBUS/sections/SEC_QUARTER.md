# SEC_QUARTER

* * * * * * * * * *

## Introduction

**SEC_QUARTER** is a constant list (`.gcf`, package `logiBUS::sections`) with the four values of a section in the condensed work state, 2 bits per section. The constants are of type `BYTE`.

## Constants

| Name | Value | Meaning |
| ---- | ----- | ------- |
| `QUARTER_OFF` | `2#00` | off |
| `QUARTER_ON` | `2#01` | on |
| `QUARTER_ERROR` | `2#10` | error |
| `QUARTER_NA` | `2#11` | not available |

## Technical Details

- The meaning of `2#10` and `2#11` depends on whether an actual value or a setpoint is meant. See [Condensed Work State](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/en/latest/Condensed-Work-State/). The constants only name the bit patterns.
- The switcher [`SEC_SWITCH_LR_QUARTER`](SEC_SWITCH_LR_QUARTER.md) uses them for its mask: free `QUARTER_ON`, blocked `QUARTER_OFF`, sections above `N` `QUARTER_NA`.

## Conclusion

`SEC_QUARTER` gives the four 2-bit values readable names.
