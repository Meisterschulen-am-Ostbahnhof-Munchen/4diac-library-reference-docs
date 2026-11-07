# logiBUS_QD_SDM_G

![logiBUS_QD_SDM_G](./logiBUS_QD_SDM_G.svg)

* * * * * * * * * *

## Introduction

The function block `logiBUS_QD_SDM_G` is a sigma-delta output like `logiBUS_QD_SDM`, but converts the value with a gamma correction for LEDs: `OUT` (0..8191) is the brightness as the eye sees it, the pulse density follows (OUT / 8191)^GAMMA. Output with 8 bit (256 steps) at 1 MHz, without ramp.

**Logic level only.** The block is meant for LEDs and LED drivers, not for the power outputs: the hardware page [Power stages of the outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/en/latest/hw/logiBUS/Leistungsstufen/) describes what sits behind the outputs. The firmware does not check this.

## Interface Structure

### **Event Inputs**

| Event | Type | Linked to / Comment |
|---|---|---|
| INIT | EInit | Service initialization; linked to QI, PARAMS, Output, GAMMA |
| REQ | Event | Service request; linked to QI, OUT |

### **Event Outputs**

| Event | Type | Linked to / Comment |
|---|---|---|
| INITO | EInit | Initialization acknowledgement; linked to QO, STATUS |
| CNF | Event | Acknowledgement of the requested service operation; linked to QO, STATUS |

### **Data Inputs**

| Variable | Type | Comment |
|---|---|---|
| QI | BOOL | Event input qualifier |
| PARAMS | STRING | Service parameters |
| OUT | DWORD | Brightness (0..8191), converted with GAMMA |
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifies the output Output_Q1..Q8; initial value `logiBUS_DO::Invalid` |
| GAMMA | REAL | Gamma of the brightness curve, read at INIT: pulse density from (OUT / 8191)^GAMMA. `1.0` = linear, `2.2` = sRGB (initial value). Invalid values (≤ 0, not finite) run linear like `1.0`. |

### **Data Outputs**

| Variable | Type | Comment |
|---|---|---|
| QO | BOOL | Event output qualifier |
| STATUS | STRING | Service status |

## Functionality

At `INIT` the output and `GAMMA` are taken over. Each `REQ` converts `OUT` to the pulse density with (OUT / 8191)^GAMMA and outputs it at once; `CNF` acknowledges. An invalid `GAMMA` (≤ 0 or not finite) runs linear like `1.0`.

## Technical Features

- Pulse density with 8 bit (256 steps), 1 MHz.
- Value range `OUT`: 0..8191 (13 bit), no ramp.
- No `FREQ` or `RAMP` parameter as with `logiBUS_QD_PWM`.
- With 8 bit the lowest steps of the gamma curve become coarse; this is stated in the comment of the block.
- Source: comments in the `.fbt` of `logiBUS_io-3.3.0`; the firmware behavior is described there and was not verified on hardware here.

## Comparison

| Block | Difference |
|---|---|
| `logiBUS_QD_PWM` / `logiBUS_QD_PWM_G` | PWM with frequency, ramp and 13 bit resolution (LEDC); also usable for power outputs as far as the power stage allows |
| `logiBUS_QD_SDM_G` | pulse density with 8 bit at 1 MHz, no ramp, logic level only |

## Application Scenarios

- Brightness control of LEDs and LED drivers at the logic output.
- Applications where a high clock rate matters more than a fine resolution.
