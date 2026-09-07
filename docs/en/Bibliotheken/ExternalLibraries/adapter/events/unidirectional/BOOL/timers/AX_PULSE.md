# AX_PULSE

* * * * * * * * * *

## Introduction

**Important Note: This function block requires only one event and no cyclic calls. It has no output ET and does not display the elapsed time.**
The AX_PULSE is a function block that outputs a pulse via an AX adapter.
![AX_PULSE](AX_PULSE.svg)

## Interface Structure

### **Adapters**

**Sockets (Inputs):**

- **REQ** (adapter::types::unidirectional::AX): Trigger.

**Plugs (Outputs):**

- **PULSE** (adapter::types::unidirectional::AX): Pulse output.

## Functionality

On REQ, PULSE is briefly activated.

## Technical Features

- Uses unidirectional adapters.

## State Overview

Pulse.

## Application Scenarios

Signaling

## ⚖️ Comparison with Similar Components

- **E_PULSE**

## 🛠️ Related Exercises

- [Exercise_020h_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Exercise_020i_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)

## Conclusion

Adapter-Based Pulse Component
