# AR_LAST_2

![AR_LAST_2](AR_LAST_2.svg)

## Introduction

The function block `AR_LAST_2` merges 2 identical `REAL` adapter signals (**IN1** to **IN2**) into one common output (**OUT**) following the **last writer wins** principle: the socket that sent an event last wins, and its value is passed to OUT immediately. This is pure arbitration by order in time, not a combination of the values.

## Interface Structure

### **Adapters**

**Input adapters (sockets):**

- **IN1**: REAL adapter (unidirectional)
- **IN2**: REAL adapter (unidirectional)

**Output adapter (plug):**

- **OUT**: REAL adapter (unidirectional), merged signal

## Functionality

The socket with the last event wins. OUT takes over its value immediately and sends the adapter event, even if the value stays the same. There is no fixed priority, only the order in time.

## Technical Features

- Generic function block without ECC (`GenericClassName` = `GEN_AR_LAST`); the behavior is implemented in the firmware (FORTE), the type description contains no algorithm. Up to version 1.0, `AR_LAST_2` was a basic FB with an ECC; version 1.1 is generic, the interface stays the same.
- Interface: sockets `IN1` and `IN2`, plug `OUT`.
- All sizes `AR_LAST_2` to `AR_LAST_9` have the same function and differ only in the number of sockets.
- Status: according to development, `AD_LAST_3` is tested on hardware (Trainer20, ESP32-S31); the other types and sizes are not.

## Application Scenarios

- Merging equivalent signal sources into one downstream adapter path, for example a manual and an automatic setpoint source.
- Arbitration between several operator stations that may write the same value.
- Simplifying networks that would otherwise wire `F_SEL`/`E_RS` inline.
