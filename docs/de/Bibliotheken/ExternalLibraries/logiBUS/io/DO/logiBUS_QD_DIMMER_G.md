# logiBUS_QD_DIMMER_G

![logiBUS_QD_DIMMER_G](./logiBUS_QD_DIMMER_G.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Dimmer like in the living room for lamps and LEDs, with gamma correction, on a 13-bit PWM output

Paket: `logiBUS::io::DQ`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization (mit Daten: `QI`, `PARAMS`, `Output`, `FREQ`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`, `GAMMA`)
-   **TOGGLE** (Typ: `Event`): Short press (BUTTON_SINGLE_CLICK of logiBUS_IE): off -> on with the stored brightness (first time START), on -> off; ramp time SOFT (mit Daten: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **DIM_START** (Typ: `Event`): Long press begins (BUTTON_LONG_PRESS_START): dim stepless towards LEVEL_MAX or LEVEL_MIN. Every long press changes the direction, the first one after switching on dims down; at the end it dims the other way. From off: on and dim up (mit Daten: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **DIM_STOP** (Typ: `Event`): Long press ends (BUTTON_LONG_PRESS_UP): stop dimming, the brightness stays and is stored (mit Daten: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **SWITCH_ON** (Typ: `Event`): Switch on directly with the stored brightness (nothing happens if already on) (mit Daten: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)
-   **SWITCH_OFF** (Typ: `Event`): Switch off directly, e.g. central off (nothing happens if already off) (mit Daten: `QI`, `DIM_TIME`, `LEVEL_MIN`, `LEVEL_MAX`, `START`, `SOFT`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm (mit Daten: `QO`, `STATUS`, `ON_STATE`, `LEVEL`)
-   **CNF** (Typ: `Event`): Confirmation of TOGGLE, DIM_START, DIM_STOP, SWITCH_ON, SWITCH_OFF (mit Daten: `QO`, `STATUS`, `ON_STATE`, `LEVEL`)
-   **RAMPO** (Typ: `Event`): Ramp finished: LEVEL_MIN or LEVEL_MAX reached while dimming, or switching on/off finished. Not after DIM_STOP (mit Daten: `QO`, `ON_STATE`, `LEVEL`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Event Input Qualifier
-   **PARAMS** (Typ: `STRING`): Service Parameters
-   **Output** (Typ: `logiBUS::io::DQ::logiBUS_DO_S`): Identify the Output Output_Q1..Q8 (Vorgabewert: `logiBUS_DO::Invalid`)
-   **FREQ** (Typ: `UDINT`): PWM frequency in Hz, read at INIT. Outputs with the same frequency share an LEDC timer (4 timers, LED blinking uses one). Power stage limits, not checked: APIXON 15 PWR Q1 (BTH50030) max. 100 Hz; BTS7030/BTT6030/BTH6070 for valves 100..200 Hz; Trainer20 without power stage (Vorgabewert: `400`)
-   **DIM_TIME** (Typ: `TIME`): Time for the whole range LEVEL_MIN..LEVEL_MAX while dimming; a shorter rest takes proportionally less time, so the speed is always the same (Vorgabewert: `T#4s`)
-   **LEVEL_MIN** (Typ: `REAL`): Lowest brightness for dimming in % (0..100) (Vorgabewert: `10.0`)
-   **LEVEL_MAX** (Typ: `REAL`): Highest brightness for dimming in % (0..100) (Vorgabewert: `100.0`)
-   **START** (Typ: `REAL`): Brightness in % for the first switching on, before anything is stored (Vorgabewert: `100.0`)
-   **SOFT** (Typ: `TIME`): Ramp time for switching on and off (TOGGLE, SWITCH_ON, SWITCH_OFF). T#0ms = jump (Vorgabewert: `T#300ms`)
-   **GAMMA** (Typ: `REAL`): Gamma of the brightness curve, read at INIT: duty = 8191 * (brightness / 8191)^GAMMA. 1.0 = linear (like logiBUS_QD_DIMMER), 2.2 = sRGB (default), 2.6 / 2.8 = Espressif examples, often more even on LEDs. Invalid values (<= 0, not finite) run linear like 1.0 (Vorgabewert: `2.2`)

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Event Output Qualifier
-   **STATUS** (Typ: `STRING`): Service Status
-   **ON_STATE** (Typ: `BOOL`): TRUE = on, FALSE as soon as switching off begins
-   **LEVEL** (Typ: `REAL`): Brightness in %: on CNF the target of switching on/off or the stopped brightness, on RAMPO the brightness reached

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

Hinweis: Diese Seite wurde aus der Typdefinition (`logiBUS_QD_DIMMER_G.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
