# SYS_SR

![SYS_SR](./SYS_SR.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Event-driven bistable

Package: `iec61499::system`

## Interface structure

### **Event inputs**

-   **S** (Type: `Event`): Set output Q
-   **R** (Type: `Event`): Reset output Q

### **Event outputs**

-   **EO** (Type: `Event`): Output Q has changed (with data: `Q`)

### **Data inputs**

None.

### **Data outputs**

-   **Q** (Type: `BOOL`): value of flip flop

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`SYS_SR.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
