# logiBUS_QDA_SDM_G

![logiBUS_QDA_SDM_G](./logiBUS_QDA_SDM_G.svg)

![logiBUS_QDA_SDM_G_network](./logiBUS_QDA_SDM_G_network.svg)

* * * * * * * * * *

## Introduction

The function block `logiBUS_QDA_SDM_G` is a composite around `logiBUS_QD_SDM_G`. It receives the brightness through an adapter (socket `OUT`, type `AD`, 0..8191 in `D1`) and `GAMMA` at INIT.

**Logic level only.** The block is meant for LEDs and LED drivers, not for the power outputs: the hardware page [Power stages of the outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/en/latest/hw/logiBUS/Leistungsstufen/) describes what sits behind the outputs. The firmware does not check this.

## Interface Structure

### **Event Inputs**

| Event | Type | Linked to / Comment |
|---|---|---|
| INIT | EInit | Service initialization; linked to QI, PARAMS, Output, GAMMA |
| *no further event input* | | *New values arrive through the adapter* |

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
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifies the output Output_Q1..Q8; initial value `logiBUS_DO::Invalid` |
| GAMMA | REAL | Gamma of the brightness curve, read at INIT: pulse density from (OUT / 8191)^GAMMA. `1.0` = linear, `2.2` = approximation of sRGB (initial value). Invalid values (≤ 0, not finite) run linear like `1.0`. |

### **Data Outputs**

| Variable | Type | Comment |
|---|---|---|
| QO | BOOL | Event output qualifier |
| STATUS | STRING | Service status |

### **Adapters**

| Type | Name | Direction | Comment |
|---|---|---|---|
| adapter::types::unidirectional::AD | OUT | Socket | Receives the event (E1) and the brightness 0..8191 (D1) |

## Functionality

The composite forwards `INIT` with `QI`, `PARAMS`, `Output` and `GAMMA` to the internal `logiBUS_QD_SDM_G` (`QX`). Event `E1` and value `D1` of the adapter `OUT` go to `QX` as `REQ` and `OUT`; `CNF` of `QX` is passed outward.

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
| `logiBUS_QDA_SDM_G` | pulse density with 8 bit at 1 MHz, no ramp, logic level only |

## Application Scenarios

- Brightness control of LEDs and LED drivers at the logic output.
- Applications where a high clock rate matters more than a fine resolution.
