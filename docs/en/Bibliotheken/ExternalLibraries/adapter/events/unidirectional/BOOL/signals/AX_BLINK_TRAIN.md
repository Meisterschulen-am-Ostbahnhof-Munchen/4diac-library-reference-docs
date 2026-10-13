# AX_BLINK_TRAIN

![AX_BLINK_TRAIN_network](./AX_BLINK_TRAIN_network.svg)

* * * * * * * * * *

## Introduction

The AX_BLINK_TRAIN is a function block that outputs a flashing signal with a limited number N of events (defined on and off times) via an AX adapter.
![AX_BLINK_TRAIN](AX_BLINK_TRAIN.svg)

## Interface Structure

### **Event Inputs**

- **START** (Event): Starts the periodic generation of events (with TIMELOW, TIMEHIGH, N).
- **STOP** (Event): Stops the generation.

### **Data Inputs**

- **TIMELOW** (TIME): Time for OUT to be FALSE.
- **TIMEHIGH** (TIME): Time for OUT to be TRUE.
- **N** (UINT): Number of events to generate.

### **Adapters**

**Plugs (Outputs):**

- **OUT** (adapter::types::unidirectional::AX): Output value, starts with FALSE and switches between TRUE and FALSE for the given high and low times.

## Functionality

After `START`, the function block outputs a flashing signal at the adapter OUT: the output starts with FALSE and is TRUE for TIMEHIGH and FALSE for TIMELOW in each cycle. N events are generated; `STOP` ends the generation.

## Technical Features

- Uses unidirectional adapters.

## State Overview

State-based.

## Application Scenarios

Railway crossings, warning signals.

## ⚖️ Comparison with similar modules

- No direct standard equivalent.

## 🛠️ Related Exercises

- [Exercise_035a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a3_AX/)

## Conclusion

Adapter-based train flashing module.
