# anlagenSequenz

![anlagenSequenz](./anlagenSequenz.svg)

* * * * * * * * * *

## Introduction

This document describes the global constants defined for the "AnlagenSequenz" (plant sequence) control logic. These constants provide fixed numeric codes for the operating states, fault states, and transition types used in a motor sequence control system, particularly for a plant with six motors.

## Interface Structure

Since this is a set of global constants, there are no event inputs, event outputs, data inputs, data outputs, or adapters in the traditional sense. The constants are directly accessible as global constants in the IEC 61499 environment.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The following constants are available as global constants:

| Constant | Type | Value | Description |
|----------|------|-------|-------------|
| STATUS_BETRIEB_AUS | SINT | SINT#0 | Anlage steht (Plant stopped) |
| STATUS_BETRIEB_HOCHFAHREN | SINT | SINT#1 | Vorlauf-Kette aktiv (Start-up chain active) |
| STATUS_BETRIEB_LAEUFT | SINT | SINT#2 | alle 6 Motoren laufen (All 6 motors running) |
| STATUS_BETRIEB_HERUNTERFAHREN | SINT | SINT#3 | Nachlauf-Kette aktiv (Shutdown chain active) |
| STATUS_STOERUNG_KEINE | SINT | SINT#0 | keine aktive Störung (No active fault) |
| STATUS_STOERUNG_AKTIV | SINT | SINT#4 | Störung aktiv, verriegelt bis EIN (Fault active, interlocked until ENABLE) |
| TRANSIT_ART_KEINE | SINT | SINT#0 | kein Motor aktuell im Vor-/Nachlauf (No motor currently in start-up/shutdown) |
| TRANSIT_ART_VORLAUF | SINT | SINT#1 | TRANSIT_MOTOR startet nach Ablauf der aktuellen Zeit (motor starts after current time expires) |
| TRANSIT_ART_NACHLAUF | SINT | SINT#2 | TRANSIT_MOTOR stoppt nach Ablauf der aktuellen Zeit (motor stops after current time expires) |

### **Adapters**

None.

## Functionality

The global constants define a standardized set of status codes and transition types used within the sequence control logic. They ensure consistent representation of the plant's operational states and fault conditions across all function blocks and devices in the system. The constants separate the operational status (STATUS_BETRIEB_*) and the fault status (STATUS_STOERUNG_*), allowing clear state handling.

## Technical Features

- Constants are defined as SINT (short integer) values.
- All constants are declared as `VAR_GLOBAL CONSTANT`, meaning they are read-only and globally accessible.
- The package name is `logiBUS::utils::sequence::const`.
- The values are chosen to avoid collisions between operational and fault status codes (fault status uses value 4, which is distinct from operational values 0-3).
- The transition types (TRANSIT_ART_*) allow identifying whether a motor is currently in a start-up (Vorlauf) or shutdown (Nachlauf) sequence.

## State Overview

While this block does not implement a state machine itself, the constants are designed to be used in state-based control. The operational states progress from `STATUS_BETRIEB_AUS` (0) through `STATUS_BETRIEB_HOCHFAHREN` (1) to `STATUS_BETRIEB_LAEUFT` (2) and then to `STATUS_BETRIEB_HERUNTERFAHREN` (3) when shutting down. A fault is represented by `STATUS_STOERUNG_AKTIV` (4), which acts as an additional state that interlocks the sequence until a new start command (EIN) is given.

## Application Scenarios

- **Plant Startup:** When the plant is commanded to start, the sequence moves from `STATUS_BETRIEB_AUS` to `STATUS_BETRIEB_HOCHFAHREN`, activating the start-up chain. The `TRANSIT_ART_VORLAUF` constant indicates that a motor is in its start-up delay.
- **Normal Operation:** After all six motors have started, the status becomes `STATUS_BETRIEB_LAEUFT`.
- **Fault Handling:** If a fault occurs, `STATUS_STOERUNG_AKTIV` is set, and the sequence is interlocked until a reset/start signal.
- **Shutdown:** During shutdown, the sequence passes through `STATUS_BETRIEB_HERUNTERFAHREN` with `TRANSIT_ART_NACHLAUF` for each motor's stop delay.

## Comparison with Similar Blocks

Since this is a global constants collection, there is no direct equivalent function block. However, compared to hard-coded values or per-block constants, this approach ensures:

- Consistency across all function blocks using the sequence logic.
- Easy maintenance if status codes need to be changed.
- Clear documentation of the meaning of each numeric value.

## Conclusion

The `anlagenSequenz` global constants provide a well-defined set of numeric codes for operating states, fault states, and transition types in a motor sequence control system. By centralizing these definitions, the system becomes more maintainable and less error-prone, ensuring that all components interpret the state information consistently.