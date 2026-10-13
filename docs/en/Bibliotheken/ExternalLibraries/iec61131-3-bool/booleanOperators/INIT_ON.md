# INIT_ON

![INIT_ON_network](./INIT_ON_network.svg)

* * * * * * * * * *

## Introduction

`INIT_ON` is the counterpart of `E_TRIG`, extended by an independent `INIT_ON`/`INITO_ON` pass-through (event type `EInitOn`). The block contains no logic; every input event is forwarded directly to the corresponding output.

![INIT_ON](INIT_ON.svg)

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
| --- | --- | --- |
| INIT | EInit | Initialization request |
| INIT_ON | EInitOn | Initialization request, own channel independent of `INIT` |
| REQ | Event | Service request |

### **Event Outputs**

| Name | Type | Comment |
| --- | --- | --- |
| INITO | EInit | Initialization confirm |
| INITO_ON | EInitOn | Initialization confirm, own channel independent of `INITO` |
| CNF | Event | Confirmation of requested service |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The block consists of event connections only:

- `INIT` → `INITO`
- `REQ` → `CNF`
- `INIT_ON` → `INITO_ON`

## Technical Details

- Pure pass-through without logic and without data
- The `INIT_ON` channel is independent of the `INIT` channel

## Application Scenarios

- Forwarding initialisation events of a dedicated channel (`EInitOn`) in a chain of blocks

## ⚖️ Comparison with Similar Building Blocks

- [INIT](./INIT.md) only contains `INIT`/`REQ` without the `INIT_ON` channel
- [INIT_OFF](./INIT_OFF.md) offers an `INIT_OFF`/`INITO_OFF` channel (`EInitOff`) instead, [INIT_ON_OFF](./INIT_ON_OFF.md) offers both channels

## Conclusion

`INIT_ON` forwards `INIT`, `INIT_ON` and `REQ` unchanged as `INITO`, `INITO_ON` and `CNF`.
