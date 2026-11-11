# logiBUS_QD_PWM

* * * * * * * * * *

## Introduction

The function block `logiBUS_QD_PWM` is an output service interface function block for double-word output data. It serves as an interface for controlling PWM (pulse-width modulation) outputs via the logiBUS system and enables the control of outputs Q1 to Q8.
![logiBUS_QD_PWM](logiBUS_QD_PWM.svg)

## Interface Structure

### **Event Inputs**

- **INIT**: Service Initialization
- Linked to: QI, PARAMS, Output, FREQ, RAMP
- **REQ**: Service Request
- Linked to: QI, OUT, RAMP

### **Event Outputs**

- **INITO**: Initialization Acknowledgement
- Linked to: QO, STATUS
- **CNF**: Acknowledgement of Requested Service Operation
- Linked to: QO, STATUS
- **RAMPO**: Ramp finished, value reached (directly after the jump with `RAMP = T#0ms`, not after INIT)
- Linked to: QO

### **Data Inputs**

- **QI** (BOOL): Event Input Qualifier
- **PARAMS** (STRING): Service Parameters
- **OUT** (DWORD): Output Data for the Resource (13-bit raw PWM value `0` to `8191`, corresponding to `0 %` to `100 %` duty cycle, $2^{13} = 8192$ states)
- **Output** (logiBUS_DO_S): Identifies the output Output_Q1..Q8
- Initial Value: `logiBUS_DO::Invalid`
- **FREQ** (UDINT): PWM frequency in Hz, read at INIT. Initial value: `400`
- **RAMP** (TIME): transition time to the new value (hardware ramp), read at INIT and REQ. Initial value: `T#0ms` (jump)

### **Data Outputs**

- **QO** (BOOL): Event Output Qualifier
- **STATUS** (STRING): Service Status

### **Adapters**

No adapter interfaces available.

## Functionality

This function block enables PWM control of outputs via the logiBUS system. During initialization (INIT), the service parameters are configured and the specific output is identified. PWM data (`OUT`, 13-bit value `0` to `8191` inside a DWORD variable) can be sent to the configured output via a REQ request. The block acknowledges both initialization and service requests via the corresponding output events.

## Frequency and ramp (from version 1.2)

- **`FREQ`** (UDINT, in Hz, default `400`): PWM frequency, read at `INIT` only. A new frequency needs a new `INIT`. Outputs with the same frequency share one LEDC timer. There are 4 timers, and LED blinking also uses one.
- **`RAMP`** (TIME, default `T#0ms`): transition time to the new value as a hardware ramp, does not block. `T#0ms` is a jump. `RAMP` is read at `INIT` and at `REQ`.
- **`RAMPO`** (event output, with `QO`): the value is reached. This is the end of the ramp, directly after the jump with `T#0ms`. No `RAMPO` follows `INIT`.
- The duty cycle stays `0` to `8191` (13 bit).
- Source of these statements: comments in the block description (`.fbt`) and `PWM_KONZEPT.md`; the firmware behavior is described there and was not verified here.

### Notes on the power stages

The firmware does not check the frequency against the power stage of the board. The following limits are only a hint (source: the comment of `FREQ` in the block and `PWM_KONZEPT.md`):

| Power stage | Hint |
|---|---|
| APIXON 15 PWR Q1 (BTH50030) | at most 100 Hz |
| BTS7030, BTT6030, BTH6070 | for valves 100 to 200 Hz |
| Trainer20 | without power stage |

Which board carries which power stage is not yet decided and is not documented here.

## Changes compared to version 1.1

- **New:** inputs `FREQ` and `RAMP`, event `RAMPO`.
- **Behavior change:** the default of `RAMP` is `T#0ms` (jump). Until now every new value was applied as a fixed 500 ms ramp. Existing programs therefore now jump to the new value. To keep the soft start, set `RAMP = T#500ms`.
- **Requirement:** The ELF `logiBUS_io-3.1.0` (`-xtensa.elf` for S3, `-riscv32.elf` for P4/S31) needs firmware with the new host symbols `elf_isr_ledc_fade_end`, `ledc_cb_register`, `ledc_fade_stop`, `ledc_get_duty`, `ledc_timer_pause`, `lroundf` and `powf`; on P4/S31 additionally `ledc_fill_multi_fade_param_list` and `ledc_set_multi_fade` (host tables: LOGIBUS `3142a703`). On older firmware the ELF does not load; loading aborts because of the missing symbol. `FORTE_ABI` stays `6`, so the difference cannot be seen from it. Always deliver firmware and ELF together. (Statement from development, not verified against the firmware.)
- **Status:** not yet tested on hardware.

## Technical Features

- **13-Bit PWM Resolution**: Uses 13-bit normalization (`0` to `8191`) stored in a `DWORD` data type (`0` = 0% duty cycle, `8191` = 100% duty cycle, scaling factor for ISO-Designer: `0.0122085215480405` or `100 / 8191`).
- Supports up to 8 outputs (Q1-Q8) via output configuration
- String-based parameter configuration for flexible service settings
- Status feedback via STRING variable for detailed error information

## State Overview

The function block has two main states:

1. **Not Initialized**: Block waits for an INIT event
2. **Initialized and Ready**: Block can process REQ requests and output PWM data

## Application Scenarios

- Control of PWM-controlled actuators (motors, heating elements)
- Control of LED lighting with brightness control
- Control of valves with proportional control
- Industrial automation applications with logiBUS hardware

## ⚖️ Comparison with Similar Blocks

Compared to simple digital output blocks, `logiBUS_QD_PWM` offers precise PWM control with 13-bit resolution (`0`–`8191` inside a DWORD). Compared to analog output blocks, it enables direct PWM control without additional conversion.

## 🛠️ Related exercises

- [Uebung_034](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034/)
- [Uebung_034a1_Q1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q1/)
- [Uebung_034a1_Q2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q2/)
- [Uebung_034a1_Q4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q4/)
- [Uebung_034b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034b/)
- [Uebung_152](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_152/)
- [Uebung_153](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_153/)

## Conclusion

The `logiBUS_QD_PWM` function block provides a powerful interface for PWM outputs in the logiBUS system. Thanks to its flexible configuration and support for 13-bit PWM data (`0`–`8191`), it is ideally suited for precise control applications in industrial automation systems.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 The PWM signal & infographic on ms-muc-docs.de](https://www.ms-muc-docs.de/automatisierung/das-pwm-signal-die-kunst-spannung-zu-zerhacken/das-pwm-signal-die-kunst-spannung-zu-zerhacken-website/)

]
