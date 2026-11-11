# ATM_LAST_7

![ATM_LAST_7](ATM_LAST_7.svg)

## Introduction

The function block `ATM_LAST_7` merges 7 identical `TIME` adapter signals (**IN1** to **IN7**) into one common output (**OUT**) following the **last writer wins** principle: the socket that sent an event last wins, and its value is passed to OUT immediately. This is pure arbitration by order in time, not a combination of the values.

## Interface Structure

### **Adapters**

**Input adapters (sockets):**

- **IN1**: TIME adapter (unidirectional)
- **IN2**: TIME adapter (unidirectional)
- **IN3**: TIME adapter (unidirectional)
- **IN4**: TIME adapter (unidirectional)
- **IN5**: TIME adapter (unidirectional)
- **IN6**: TIME adapter (unidirectional)
- **IN7**: TIME adapter (unidirectional)

**Output adapter (plug):**

- **OUT**: TIME adapter (unidirectional), merged signal

## Functionality

The socket with the last event wins. OUT takes over its value immediately and sends the adapter event, even if the value stays the same. There is no fixed priority, only the order in time.

## Technical Features

- Generic function block without ECC (`GenericClassName` = `GEN_ATM_LAST`); the behavior is implemented in the firmware (FORTE), the type description contains no algorithm.
- Interface: sockets `IN1` to `IN7`, plug `OUT`.
- All sizes `ATM_LAST_2` to `ATM_LAST_9` have the same function and differ only in the number of sockets.
- The library version stays 3.0.0. A new FORTE or `adapter_events` ELF is required; otherwise `ATM_LAST_3` and above counts as an unknown type (statement from development, not verified).
- Status: according to development, `AD_LAST_3` is tested on hardware (Trainer20, ESP32-S31); the other types and sizes are not.

## Application Scenarios

- Merging equivalent signal sources into one downstream adapter path, for example a manual and an automatic setpoint source.
- Arbitration between several operator stations that may write the same value.
- Simplifying networks that would otherwise wire `F_SEL`/`E_RS` inline.
