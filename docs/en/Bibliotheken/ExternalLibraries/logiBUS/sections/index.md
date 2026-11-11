# sections

## Introduction

The package `logiBUS::sections` belongs to two libraries ([PR #347](https://github.com/Meisterschulen-am-Ostbahnhof-Munchen/4diac_training1/pull/347)), both of which depend only on `core` and can therefore be used without ISOBUS:

- `logiBUS_sections_bit`: 1 bit per section, 1 = free.
- `logiBUS_sections_quarter`: 2 bits per section (condensed work state).

The occasion was the section switcher of a cereal hoe: four keys switch sections off and on again from the left or right. Section 1 is on the left and occupies the least significant bits.

## Library `logiBUS_sections_bit`

| Block | `MASK` | Sections |
| ----- | ------ | -------- |
| [`SEC_SWITCH_LR_BIT`](SEC_SWITCH_LR_BIT.md) | `ARRAY[0..7] OF DWORD` | 256 |
| [`SEC_SWITCH_LR_BIT_WORD`](SEC_SWITCH_LR_BIT_WORD.md) | `WORD` | 16 |
| [`SEC_SWITCH_LR_BIT_DWORD`](SEC_SWITCH_LR_BIT_DWORD.md) | `DWORD` | 32 |
| [`SEC_SWITCH_LR_BIT_LWORD`](SEC_SWITCH_LR_BIT_LWORD.md) | `LWORD` | 64 |

## Library `logiBUS_sections_quarter`

| Block | `MASK` | Sections |
| ----- | ------ | -------- |
| [`SEC_SWITCH_LR_QUARTER`](SEC_SWITCH_LR_QUARTER.md) | `ARRAY[0..15] OF DWORD` | 256 |
| [`SEC_SWITCH_LR_QUARTER_WORD`](SEC_SWITCH_LR_QUARTER_WORD.md) | `WORD` | 8 |
| [`SEC_SWITCH_LR_QUARTER_DWORD`](SEC_SWITCH_LR_QUARTER_DWORD.md) | `DWORD` | 16 |
| [`SEC_SWITCH_LR_QUARTER_LWORD`](SEC_SWITCH_LR_QUARTER_LWORD.md) | `LWORD` | 32 |

In addition:

| Block | Purpose |
| ----- | ------- |
| [`SEC_MASK_QUARTER_CMD`](SEC_MASK_QUARTER_CMD.md) | Block a setpoint with a BIT mask: blocked is always `00` |
| [`SEC_MASK_QUARTER_ACT`](SEC_MASK_QUARTER_ACT.md) | Block an actual value with a BIT mask: only `01` becomes `00`, `10` and `11` remain |
| [`SEC_QUARTER`](SEC_QUARTER.md) | Constants `QUARTER_OFF`, `QUARTER_ON`, `QUARTER_ERROR`, `QUARTER_NA` |

Word k of the QUARTER switcher corresponds to word k of the TC DDI.

## Common Behavior of the Switchers

| Event | Effect |
| ----- | ------ |
| `INIT+` | take over `N`, counters 0, `INITO+`. In the active state initialize again. |
| `INIT-` | deactivated, mask all free, `INITO-` (also before `INIT+`) |
| `REQ+` / `REQ-` | output the mask again, `CNF+` / change nothing, `CNF-` |
| `LEFT_OFF_INC/DEC`, `RIGHT_OFF_INC/DEC` | switch one section off or on from the left or right, `CNF+`. At the limit no reaction. |
| `ALL_ON` / `ALL_OFF` | all free / all blocked (`LEFT_OFF` = `N`), `CNF+` |

- `N` and `QI` are connected by `WITH` only to `INIT`, `QI` additionally to `REQ`.
- After `ALL_OFF` both sides switch on symmetrically (since version 1.1, PR #349): `LEFT_OFF_DEC` starts at the right section, `RIGHT_OFF_DEC` at the left one.
- The service sequences (17 per switcher) are shown as sequence diagrams on the type pages.

## Two Mask Blocks

`11` means "no change" for the setpoint and "not installed" for the actual value (ISO 11783-11, DDI 290 and 161). If a blocked section passed `11` through for the setpoint, it would stay on after an earlier `01`. Hence `SEC_MASK_QUARTER_CMD` and `SEC_MASK_QUARTER_ACT`. Background and combination rules: [Condensed Work State](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/en/latest/Condensed-Work-State/).

## Status

- The libraries are in `develop` of `4diac_training1`.
- They are not yet tested in FORTE and have no export/ELF module yet (as of the PR description).
