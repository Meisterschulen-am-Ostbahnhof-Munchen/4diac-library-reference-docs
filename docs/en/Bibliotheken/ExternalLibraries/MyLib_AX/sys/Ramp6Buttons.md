# Ramp6Buttons

![Ramp6Buttons_network](./Ramp6Buttons_network.svg)

* * * * * * * * * *

## Introduction

`Ramp6Buttons` encapsulates the **7 VT buttons of a PWM channel** — the 6 ramp buttons (`0 -- - + ++ F`) plus the channel enable/disable switch — as its own reusable SubApp. It was extracted from [`RampLimitFS_TO_logiBUS_QDA_PWM_OPC`](./RampLimitFS_TO_logiBUS_QDA_PWM_OPC.md) to declutter that block's network and make the button logic independently reusable.

![Ramp6Buttons](Ramp6Buttons.svg)

## Interface

- **Event input**: `INIT` (`EInit`, Initialization Request)
- **Event outputs**: `INITO` (`EInit`, Initialization Confirm), `IND_SWITCH`, `IND_ZERO`, `IND_UP_SLOW`, `IND_UP_FAST`, `IND_DOWN_SLOW`, `IND_DOWN_FAST`, `IND_FULL`
- **Data inputs**: `QI` (`BOOL`, default `TRUE`, Quality Input) as well as `u16ObjId_SWITCH`, `u16ObjId_ZERO`, `u16ObjId_UP_SLOW`, `u16ObjId_UP_FAST`, `u16ObjId_DOWN_SLOW`, `u16ObjId_DOWN_FAST`, `u16ObjId_FULL`
- **Data output**: `QO` (`BOOL`, Quality Output)

## Function Blocks (FBs) Used

### Sub-blocks: Ramp6Buttons

- **Type**: SubAppType
- **Internal FBs used**:
    - **Button_SWITCH**, **Button_ZERO**, **Button_UP_SLOW**, **Button_UP_FAST**, **Button_DOWN_SLOW**, **Button_DOWN_FAST**, **Button_FULL**: each `isobus::UT::io::Button::Button_IE`
        - Parameter: `InputEvent=BT_PRESSED_LATCHED` (a latched press/release event, no adapter bridging via `AX_RF_TRIG` needed)
        - Data inputs: `u16ObjId` (object ID of the respective VT button), `QI` (from the SubApp input `QI`)
        - Event output: `IND` (button pressed)
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_7` — ANDs the `QO` outputs of all seven `Button_*` instances into the SubApp output `QO`
- **Functionality**: Seven identical `Button_IE` instances, each bound to its own VT object ID, pass their `IND` event straight through as their own SubApp output event — a pure 1:1 pass-through with no additional logic. In addition, the initialization is chained through all seven instances and the status `QO` is aggregated.

## Program Flow and Connections

The same pattern applies to all 7 buttons (`u16ObjId_<NAME>` → `Button_<NAME>.u16ObjId`, `Button_<NAME>.IND` → `IND_<NAME>`):

- `u16ObjId_SWITCH` → `Button_SWITCH.u16ObjId`; `Button_SWITCH.IND` → `IND_SWITCH` (toggles channel enable)
- `u16ObjId_ZERO` → `Button_ZERO.u16ObjId`; `Button_ZERO.IND` → `IND_ZERO` (`RampLimitFS.ZERO`)
- `u16ObjId_UP_SLOW` → `Button_UP_SLOW.u16ObjId`; `Button_UP_SLOW.IND` → `IND_UP_SLOW` (`RampLimitFS.UP_SLOW`, ~1 %)
- `u16ObjId_UP_FAST` → `Button_UP_FAST.u16ObjId`; `Button_UP_FAST.IND` → `IND_UP_FAST` (`RampLimitFS.UP_FAST`, ~10 %)
- `u16ObjId_DOWN_SLOW` → `Button_DOWN_SLOW.u16ObjId`; `Button_DOWN_SLOW.IND` → `IND_DOWN_SLOW` (`RampLimitFS.DOWN_SLOW`, ~1 %)
- `u16ObjId_DOWN_FAST` → `Button_DOWN_FAST.u16ObjId`; `Button_DOWN_FAST.IND` → `IND_DOWN_FAST` (`RampLimitFS.DOWN_FAST`, ~10 %)
- `u16ObjId_FULL` → `Button_FULL.u16ObjId`; `Button_FULL.IND` → `IND_FULL` (`RampLimitFS.FULL`, 100 %)

In addition:

- Initialization chain: `INIT` → `Button_SWITCH.INIT`, `Button_SWITCH.INITO` → `Button_ZERO.INIT`, `Button_ZERO.INITO` → `Button_UP_SLOW.INIT`, `Button_UP_SLOW.INITO` → `Button_UP_FAST.INIT`, `Button_UP_FAST.INITO` → `Button_DOWN_SLOW.INIT`, `Button_DOWN_SLOW.INITO` → `Button_DOWN_FAST.INIT`, `Button_DOWN_FAST.INITO` → `Button_FULL.INIT`, `Button_FULL.INITO` → `INITO` and → `AND_QO.REQ`
- `QI` → `QI` of all seven `Button_*` instances
- `Button_SWITCH.QO`, `Button_ZERO.QO`, `Button_UP_SLOW.QO`, `Button_UP_FAST.QO`, `Button_DOWN_SLOW.QO`, `Button_DOWN_FAST.QO`, `Button_FULL.QO` → `AND_QO.IN1`..`IN7`; `AND_QO.OUT` → `QO`

The calling block wires `IND_ZERO`/`IND_UP_SLOW`/`IND_UP_FAST`/`IND_DOWN_SLOW`/`IND_DOWN_FAST`/`IND_FULL` directly to the like-named event inputs of `RampLimitFS`, and `IND_SWITCH` to the channel enable flip-flop.

## Technical Details

- **`BT_PRESSED_LATCHED` instead of `Button_IXA`+`AX_RF_TRIG`**: a simpler pattern with no adapter bridging, since `Button_IE` outputs the press event directly as `IND`.
- **Event order aligned with `RampLimitFS`**: The output events (`SWITCH, ZERO, UP_SLOW, UP_FAST, DOWN_SLOW, DOWN_FAST, FULL`) deliberately follow the order in which `RampLimitFS` declares its own event inputs, to make wiring easier.

## Application Scenarios

- Any exercise with a ramped setpoint (`RampLimitFS` or similar) that should be operated via 6 buttons plus one enable switch.

## Summary

`Ramp6Buttons` bundles seven identical `Button_IE` instances into a single reusable SubApp, keeping the button wiring out of the actual PWM channel block.

## 🛠️ Related Exercises

- [RampLimitFS_TO_logiBUS_QDA_PWM_OPC](./RampLimitFS_TO_logiBUS_QDA_PWM_OPC.md)
- [InputOutputTesterButton_PWM_OPC_UA](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_PWM_OPC_UA/InputOutputTesterButton_PWM_OPC_UA/)

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
