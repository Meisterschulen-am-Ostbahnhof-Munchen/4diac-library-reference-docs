# logiBUS_AI_MV

![logiBUS_AI_MV](./logiBUS_AI_MV.svg)

* * * * * * * * * *

## Introduction

`logiBUS_AI_MV` is a service interface function block for one analog input in millivolts. It works like [`logiBUS_AI_ID`](logiBUS_AI_ID.md), but `IN` delivers the calibrated voltage at the pin instead of the raw value.

## Interface structure

### **Event inputs**

| Event | Type  | With                                                                | Description                   |
| ----- | ----- | ------------------------------------------------------------------- | ----------------------------- |
| INIT  | EInit | QI, PARAMS, Input, AnalogInput_hysteresis, TimeDelta, TimeRateLimit | Service initialization        |
| REQ   | Event | QI                                                                  | Service request (immediate read) |

### **Event outputs**

| Event | Type  | With           | Description                                  |
| ----- | ----- | -------------- | -------------------------------------------- |
| INITO | EInit | QO, STATUS     | Initialization confirmation                  |
| CNF   | Event | QO, STATUS, IN | Confirmation of the requested service        |
| IND   | Event | QO, STATUS, IN | Indication from the resource                 |

### **Data inputs**

| Name                   | Type                          | Initial value | Description                                                                          |
| ---------------------- | ----------------------------- | ------------- | ------------------------------------------------------------------------------------ |
| QI                     | BOOL                          | –             | Event input qualifier                                                                |
| PARAMS                 | STRING                        | –             | Service parameters                                                                   |
| Input                  | logiBUS::io::AI::logiBUS_AI_S | Invalid       | Selects the input (I1..I8)                                                           |
| AnalogInput_hysteresis | DWORD                         | –             | IND when the voltage changes by more than this many mV (0 = off, 1 = every change)   |
| TimeDelta              | DWORD                         | 250           | Cyclic IND after at most this many ms (0 or 16#FFFFFFFF = off)                       |
| TimeRateLimit          | DWORD                         | 100           | At most one IND per this many ms (less than TimeDelta)                               |

### **Data outputs**

| Name   | Type   | Description                                              |
| ------ | ------ | -------------------------------------------------------- |
| QO     | BOOL   | Event output qualifier                                   |
| STATUS | STRING | Service status                                           |
| IN     | DWORD  | Input data: calibrated voltage at the pin in millivolts  |

### **Adapters**

No adapter interfaces.

## How it works

`IN` is the voltage at the chip pin in **millivolts**. The firmware converts the averaged raw value with the chip calibration (ESP-IDF `adc_cali`, eFuse values). The attenuation is the same as for `logiBUS_AI_ID`: ESP32-S3 about 0-2900 mV (±50 mV), ESP32-P4 about 0-3300 mV (±15 mV), according to the datasheet.

The voltage at the terminal depends on the voltage divider of the board and is not included.

Hysteresis and timing work as for `logiBUS_AI_ID`, except that `AnalogInput_hysteresis` is in mV:

- `AnalogInput_hysteresis = 0` switches the change trigger off (with `1` every change is reported).
- `TimeDelta = 0` or `16#FFFFFFFF` switches the cyclic IND off.
- With both off, the block sends no IND, the value only comes via REQ.

## Technical notes

- **Calibration required:** Without a calibration for the channel, INIT returns `QO` = FALSE and `STATUS` = `ADC calibration not available`. This applies to the ESP32-S31: ESP-IDF 6.1 has no calibration for it yet. On ESP32-S3 and ESP32-P4 the block works.
- **Unit:** `IN` and `AnalogInput_hysteresis` are both in mV. For the raw value (0-4095) use [`logiBUS_AI_ID`](logiBUS_AI_ID.md).

## Comparison with similar blocks

- **[`logiBUS_AI_ID`](logiBUS_AI_ID.md):** Same interface, but `IN` is the raw value and the hysteresis applies to the raw value.
- **[`logiBUS_AI_MVA`](logiBUS_AI_MVA.md):** Composite around this block with adapter output `IN` and external request socket `SREQ`.
