# Heben_Senken_TO_QDA_PWM_OPC


![Heben_Senken_TO_QDA_PWM_OPC_network](./Heben_Senken_TO_QDA_PWM_OPC_network.svg)

![Heben_Senken_TO_QDA_PWM_OPC](./Heben_Senken_TO_QDA_PWM_OPC.svg)

* * * * * * * * * *

## Introduction

Heben_Senken_TO_QDA_PWM_OPC is a composite subapplication that integrates two independent subapplications to provide a complete control solution for a PVEA actuator with two physical channels. It combines a PWM generation module (HebenSenken_ILOCK_QDA_PWM_OPC) for lifting/lowering commands with a digital output toggle module (DO_TOGGLE_RPC_QXA_OPC) for an enable signal. The subapplication is designed for systems where the module itself has no local visualization terminal, allowing full operation via remote commands, I/O tests, and an RPC‑triggered enable toggle.

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Name | Type | Description |
|------|------|-------------|
| `Output_PWM` | `logiBUS::io::DQ::logiBUS_DO_S` | Physical PWM output (left channel): 25% / 50% / 75% duty cycle, ratiometric. |
| `Output_DO`   | `logiBUS::io::DQ::logiBUS_DO_S` | Physical enable output (right channel): click‑toggle, default ON. |
| `DT_PROTECT`  | `TIME` | Protection dead time before direction change, default `T#300ms`. |
| `ID_HEBEN_READ` | `WSTRING` | Actual function command "raise" (subscribe). |
| `ID_HEBEN_WRITE` | `WSTRING` | Actual function feedback "raise", behind the ILOCK (publish). |
| `ID_SENKEN_READ` | `WSTRING` | Actual function command "lower" (subscribe). |
| `ID_SENKEN_WRITE` | `WSTRING` | Actual function feedback "lower", behind the ILOCK (publish). |
| `ID_TEST_READ_HEBEN` | `WSTRING` | Existing IO‑test subscribe address for raise (e.g., `STG2_Q0x_READ`), OR‑combined with `ID_HEBEN_READ` before the ILOCK. |
| `ID_TEST_WRITE_HEBEN` | `WSTRING` | Existing IO‑test publish address for raise, behind the ILOCK (e.g., `STG2_Q0x_WRITE`). |
| `ID_TEST_READ_SENKEN` | `WSTRING` | Existing IO‑test subscribe address for lower, OR‑combined with `ID_SENKEN_READ` before the ILOCK. |
| `ID_TEST_WRITE_SENKEN` | `WSTRING` | Existing IO‑test publish address for lower, behind the ILOCK. |
| `ID_DO_TOGGLE_METHOD` | `WSTRING` | Local method address (`ACTION=CREATE_METHOD`) for the argument‑less enable toggle trigger – called from STG1 (softkey relay) and directly from the OPC dashboard (`CALL_METHOD`). |
| `ID_DO_STATE_WRITE` | `WSTRING` | Local publish address (`ACTION=WRITE`) for the actual enable state (`AX_T_FF_INIT.Q`) – subscribed remotely from VT and dashboard. |

### **Data Outputs**

None. The subapplication does not expose explicit output variables; physical outputs are controlled directly through the `Output_PWM` and `Output_DO` references.

### **Adapters**

None.

## Functionality

The subapplication collects remote raise/lower commands and OR‑combines them with IO‑test signals. The internal PWM subapplication (`HebenSenken_ILOCK_QDA_PWM_OPC`) generates a single ratiometric PWM signal with a duty cycle of 25% (lower), 50% (neutral), and 75% (raise), incorporating an interlock and a protection dead time. The second internal subapplication (`DO_TOGGLE_RPC_QXA_OPC`) manages a digital output that can be toggled via an RPC method, defaulting to ON. The two modules operate independently but are combined here to provide a unified interface for a PVEA actuator with two physical channels.

## Technical Features

- **Composite structure**: Encapsulates two purpose‑built subapplications into a single logical block.
- **Remote operation**: Supports Subscribe/Publish for command and state feedback.
- **IO‑test integration**: Test signals are OR‑combined with actual commands, enabling commissioning and maintenance overrides.
- **PWM generation**: Includes a programmable protection dead time (`DT_PROTECT`) to prevent rapid direction changes.
- **Enable toggle**: A latchable output (default ON) controlled via an RPC method, suitable for emergency stop or street mode.
- **Generic applicability**: Works with any PVEA actuator having two physical channels (left = PWM, right = enable), independent of a local visualization terminal.

## State Overview

The subapplication itself does not implement an external state machine; the internal subapplications are responsible for state handling. The PWM module likely has states for neutral, raise, lower, and protection delay. The DO toggle module uses a flip‑flop (`AX_T_FF_INIT`) to maintain its ON/OFF state. The composite ensures that the physical outputs reflect the combined behavior.

## Application Scenarios

- **Centralized control**: Operate a Danfoss PVEA actuator lifting/lowering from a main control station without module‑local visualization.
- **Remote enable control**: Toggle the enable output from a dashboard or a VT using the RPC method, e.g., to activate a street mode or an emergency stop.
- **IO testing**: Use the integrated IO‑test signals to validate PWM and enable outputs during commissioning.
- **Generic usage**: Apply to any system requiring a PWM drive plus a latching enable output, with configurable dead time and command IDs.

## Comparison with Similar Blocks

- **Integrated vs. separate blocks**: This composite combines two essential functions (PWM control and enable toggle) in one interface, whereas separate FBs would require explicit wiring between two blocks.
- **Unified channel handling**: Directly maps the two physical channels (left PWM, right enable) to a single input set, simplifying application engineering.
- **RPC‑based toggle**: Unlike simple DO control blocks, the enable output includes a latchable flip‑flop triggered via an RPC method, offering more flexible remote operation.

## Conclusion

Heben_Senken_TO_QDA_PWM_OPC is a composite subapplication that encapsulates the complexity of controlling a PVEA actuator with a PWM signal and a toggleable enable output. It is generic, configurable via data inputs, and provides a clean interface for modules without a local visualization, enabling remote operation, IO testing, and flexible enable control.
