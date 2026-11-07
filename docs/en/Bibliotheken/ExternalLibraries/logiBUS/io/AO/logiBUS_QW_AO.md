# logiBUS_QW_AO

![logiBUS_QW_AO](./logiBUS_QW_AO.svg)

* * * * * * * * * *

## Introduction

The function block `logiBUS_QW_AO` is an output service interface for an **analog voltage** over the sigma-delta modulator and an RC low pass. `OUT` (`WORD`) is the wanted voltage in mV after the low pass, the pulse density runs at 10 MHz.

**Requirement:** an RC filter must be present on the board or on an add-on board. The block is not meant for the power outputs.

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
| OUT | WORD | Voltage in mV after the RC low pass: VDD_IO / 256 · density + VDD_IO / 2, with VDD_IO taken as 3300 mV. Range 0..3287 mV in 256 steps (about 13 mV). Higher values are limited, `STATUS` says so. |
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifies the output Output_Q1..Q8; initial value `logiBUS_DO::Invalid` |

### **Data Outputs**

| Variable | Type | Comment |
|---|---|---|
| QO | BOOL | Event output qualifier |
| STATUS | STRING | Service status |

## Functionality

At `INIT` the output is taken over. Each `REQ` converts the voltage `OUT` to a pulse density (8 bit) and outputs it. Values above 3287 mV are limited to that value, `STATUS` says so. `CNF` acknowledges.

## Technical Features

- Resolution 256 steps, about 13 mV; range 0..3287 mV.
- VDD_IO is taken as 3300 mV, not measured.
- The real voltage depends on the RC filter and the load.
- Source: comments in the `.fbt` of `logiBUS_io-3.3.0`; the firmware behavior is described there and was not verified on hardware here.

## Comparison

| Block | Difference |
|---|---|
| `logiBUS_QD_SDM` | same pulse density, but value 0..8191 and 1 MHz; no voltage value in mV |

## Application Scenarios

- Simple analog output (setpoint, auxiliary voltage) with an RC filter at the logic output.
- Voltage setpoints in mV for downstream electronics.
