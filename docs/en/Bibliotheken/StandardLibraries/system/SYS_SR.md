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

## Operation (from the source)

This description is derived from the type definition and shows states, transitions and algorithms. It has not been tested on hardware.

### Execution control chart (ECC)

-   **START** (Action: no action)
-   **SET** (Action: `SET` → sends event `EO`)
-   **RESET** (Action: `RESET` → sends event `EO`)

**Transitions**

-   START → SET, when `S`
-   SET → RESET, when `R`
-   RESET → SET, when `S`

### Algorithms

**SET**: SET algorithm

```iecst
Q := TRUE;
```

**RESET**: RESET algorithm

```iecst
Q := FALSE;
```

Note: This page was generated from the type definition (`SYS_SR.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
