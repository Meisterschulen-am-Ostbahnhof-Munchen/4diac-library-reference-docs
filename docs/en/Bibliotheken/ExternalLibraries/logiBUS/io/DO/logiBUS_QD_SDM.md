# logiBUS_QD_SDM

![logiBUS_QD_SDM](./logiBUS_QD_SDM.svg)

* * * * * * * * * *

## Introduction

The function block `logiBUS_QD_SDM` is an output service interface for a **sigma-delta output** (pulse density modulation). The value `OUT` (`DWORD`, 0..8191, 13 bit like the PWM blocks) is output as a pulse density with 8 bit (256 steps) at 1 MHz. There is no ramp: the new value applies at once.

**Logic level only.** The block is meant for LEDs and LED drivers, not for the power outputs: the hardware page [Power stages of the outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/en/latest/hw/logiBUS/Leistungsstufen/) describes what sits behind the outputs. The firmware does not check this.

## Interface Structure

### **Event Inputs**

| Event | Type | Linked to / Comment |
|---|---|---|
| INIT | EInit | Service initialization; linked to QI, PARAMS, Output |
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
| OUT | DWORD | Value 0..8191 (13 bit), output as pulse density with 8 bit |
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifies the output Output_Q1..Q8; initial value `logiBUS_DO::Invalid` |

### **Data Outputs**

| Variable | Type | Comment |
|---|---|---|
| QO | BOOL | Event output qualifier |
| STATUS | STRING | Service status |

## Functionality

At `INIT` the output is taken over. Each `REQ` sets a new value `OUT`; it is output as a pulse density at once and acknowledged with `CNF`.

## Technical Features

- Pulse density with 8 bit (256 steps), 1 MHz.
- Value range `OUT`: 0..8191 (13 bit), no ramp.
- No `FREQ` or `RAMP` parameter as with `logiBUS_QD_PWM`.
- Source: comments in the `.fbt` of `logiBUS_io-3.3.0`; the firmware behavior is described there and was not verified on hardware here.

## Comparison

| Block | Difference |
|---|---|
| `logiBUS_QD_PWM` / `logiBUS_QD_PWM_G` | PWM with frequency, ramp and 13 bit resolution (LEDC); also usable for power outputs as far as the power stage allows |
| `logiBUS_QD_SDM` | pulse density with 8 bit at 1 MHz, no ramp, logic level only |

## Application Scenarios

- Brightness control of LEDs and LED drivers at the logic output.
- Applications where a high clock rate matters more than a fine resolution.
