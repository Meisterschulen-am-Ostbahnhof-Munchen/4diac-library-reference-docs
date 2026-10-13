# AX_PULSE

![AX_PULSE_network](./AX_PULSE_network.svg)

* * * * * * * * * *

## Introduction

**Important Note: This function block requires only one event and no cyclic calls. It has no output ET and does not display the elapsed time.**
The AX_PULSE is a function block that outputs a pulse via an AX adapter.
![AX_PULSE](AX_PULSE.svg)

## Interface Structure

### **Event Inputs**

- **REQ** (Event): Service request (with `PT`); triggers the pulse.
- **R** (Event): Reset.

### **Data Inputs**

- **PT** (TIME): Pulse time.

### **Adapters**

**Plugs (Outputs):**

- **Q** (adapter::types::unidirectional::AX): Timer output (AX adapter), delivers the pulse.

## Functionality

On REQ, Q is activated for the duration `PT` (non-retriggerable); R resets the pulse. Internally an `E_PULSE` is used, whose `CNF`/`Q` are routed to the adapter `Q`.

## Technical Features

- Uses unidirectional adapters.

## State Overview

Pulse.

## Application Scenarios

Signaling

## ⚖️ Comparison with Similar Components

- **E_PULSE**

## 🛠️ Related Exercises

- [Exercise_020h_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Exercise_020i_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)

## Conclusion

Adapter-Based Pulse Component
