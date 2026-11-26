# E_DEMUX_3

![E_DEMUX_3](./E_DEMUX_3.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Event demultiplexer (generic FB)

Package: `iec61499::events`

## Interface structure

### **Event inputs**

-   **EI** (Type: `Event`): Event to demultiplex (with data: `K`)

### **Event outputs**

-   **EO1** (Type: `Event`): Output, demultiplexed from EI when K=0
-   **EO2** (Type: `Event`): Output, demultiplexed from EI when K=1
-   **EO3** (Type: `Event`): Output, demultiplexed from EI when K=2

### **Data inputs**

-   **K** (Type: `UINT`): Event index

### **Data outputs**

None.

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`E_DEMUX_3.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
