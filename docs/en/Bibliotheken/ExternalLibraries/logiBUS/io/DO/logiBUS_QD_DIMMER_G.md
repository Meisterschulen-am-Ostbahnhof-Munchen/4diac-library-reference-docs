# logiBUS_QD_DIMMER_G

![logiBUS_QD_DIMMER_G](./logiBUS_QD_DIMMER_G.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Dimmer like in the living room for lamps and LEDs, with gamma correction, on a 13-bit PWM output

Package: `logiBUS::io::DQ`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization (with data: `QI`, `PARAMS`, `Output`, `FREQ`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`, `GAMMA`)
-   **TOGGLE** (Type: `Event`): Short press (BUTTON_SINGLE_CLICK of logiBUS_IE): off -> on with the stored brightness (first time START), on -> off; ramp time SOFT (with data: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **DIM_START** (Type: `Event`): Long press begins (BUTTON_LONG_PRESS_START): dim stepless towards LEVEL_MAX or LEVEL_MIN. Every long press changes the direction, the first one after switching on dims down; at the end it dims the other way. From off: on and dim up (with data: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **DIM_STOP** (Type: `Event`): Long press ends (BUTTON_LONG_PRESS_UP): stop dimming, the brightness stays and is stored (with data: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **SWITCH_ON** (Type: `Event`): Switch on directly with the stored brightness (nothing happens if already on) (with data: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **SWITCH_OFF** (Type: `Event`): Switch off directly, e.g. central off (nothing happens if already off) (with data: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm (with data: `QO`, `STATUS`, `ON_STATE`, `LEVEL`)
-   **CNF** (Type: `Event`): Confirmation of TOGGLE, DIM_START, DIM_STOP, SWITCH_ON, SWITCH_OFF (with data: `QO`, `STATUS`, `ON_STATE`, `LEVEL`)
-   **RAMPO** (Type: `Event`): Ramp finished: LEVEL_MIN or LEVEL_MAX reached while dimming, or switching on/off finished. Not after DIM_STOP (with data: `QO`, `ON_STATE`, `LEVEL`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Event Input Qualifier
-   **PARAMS** (Type: `STRING`): Service Parameters
-   **Output** (Type: `logiBUS::io::DQ::logiBUS_DO_S`): Identify the Output Output_Q1..Q8 (Default: `logiBUS_DO::Invalid`)
-   **FREQ** (Type: `UDINT`): PWM frequency in Hz, read at INIT. Outputs with the same frequency share an LEDC timer (4 timers, LED blinking uses one). Power stage limits, not checked: APIXON 15 PWR Q1 (BTH50030) max. 100 Hz; BTS7030/BTT6030/BTH6070 for valves 100..200 Hz; Trainer20 without power stage (Default: `400`)
-   **DIM_TIME** (Type: `TIME`): Time for the whole range LEVEL_MIN..LEVEL_MAX while dimming; a shorter rest takes proportionally less time, so the speed is always the same (Default: `T#4s`)
-   **LEVEL_MIN** (Type: `REAL`): Lowest brightness for dimming in % (0..100) (Default: `10.0`)
-   **LEVEL_MAX** (Type: `REAL`): Highest brightness for dimming in % (0..100) (Default: `100.0`)
-   **START** (Type: `REAL`): Brightness in % for the first switching on, before anything is stored (Default: `100.0`)
-   **SOFT** (Type: `TIME`): Ramp time for switching on and off (TOGGLE, SWITCH_ON, SWITCH_OFF). T#0ms = jump (Default: `T#300ms`)
-   **GAMMA** (Type: `REAL`): Gamma of the brightness curve, read at INIT: duty = 8191 * (brightness / 8191)^GAMMA. 1.0 = linear (like logiBUS_QD_DIMMER), 2.2 = sRGB (default), 2.6 / 2.8 = Espressif examples, often more even on LEDs. Invalid values (<= 0, not finite) run linear like 1.0 (Default: `2.2`)

### **Data outputs**

-   **QO** (Type: `BOOL`): Event Output Qualifier
-   **STATUS** (Type: `STRING`): Service Status
-   **ON_STATE** (Type: `BOOL`): TRUE = on, FALSE as soon as switching off begins
-   **LEVEL** (Type: `REAL`): Brightness in %: on CNF the target of switching on/off or the stopped brightness, on RAMPO the brightness reached

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`logiBUS_QD_DIMMER_G.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
