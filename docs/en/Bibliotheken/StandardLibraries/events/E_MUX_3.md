# E_MUX_3

![E_MUX_3](./E_MUX_3.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Event multiplexer (generic FB)

Package: `iec61499::events`

## Interface structure

### **Event inputs**

-   **EI1** (Type: `Event`): Event to multiplex, K=0
-   **EI2** (Type: `Event`): Event to multiplex, K=1
-   **EI3** (Type: `Event`): Event to multiplex, K=2

### **Event outputs**

-   **EO** (Type: `Event`): Event which was multiplexed (with data: `K`)

### **Data inputs**

None.

### **Data outputs**

-   **K** (Type: `UINT`): Event index

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`E_MUX_3.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
