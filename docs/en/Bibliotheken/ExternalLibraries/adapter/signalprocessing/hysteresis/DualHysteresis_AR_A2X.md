# DualHysteresis_AR_A2X


![DualHysteresis_AR_A2X_ecc](./DualHysteresis_AR_A2X_ecc.svg)

![DualHysteresis_AR_A2X](./DualHysteresis_AR_A2X.svg)

* * * * * * * * * *

## Introduction

The **DualHysteresis_AR_A2X** function block implements a two-way analog-to-digital conversion with configurable hysteresis and deadband. It monitors an analog input value and generates discrete UP/DOWN signals based on the relationship between the input and a configurable center point (MI). The block distinguishes between a switch-on threshold (wider due to hysteresis) and a switch-off threshold (narrower due to deadband only), providing clean and stable digital output transitions without chatter around the switching points. The outputs are provided via a single unidirectional A2X adapter interface, combining both event and data signals for UP and DOWN states.

## Interface Structure

### **Event Inputs**

| Event    | Type   | Comment                     |
|----------|--------|-----------------------------|
| `INIT`   | EInit  | Initialization Request.     |

### **Event Outputs**

| Event    | Type   | Comment                     |
|----------|--------|-----------------------------|
| `INITO`  | EInit  | Initialization Confirm.     |

### **Data Inputs**

| Data | Type   | Comment              |
|------|--------|----------------------|
| `QI` | BOOL   | Input event qualifier. When `QI = TRUE`, normal processing is enabled. When `FALSE`, the block operates in safe state. |

### **Data Outputs**

| Data | Type   | Comment |
|------|--------|---------|
| `QO` | BOOL   | Output qualifier reflecting the last accepted `QI` value during operation. |

### **Adapters**

| Direction | Name       | Type                              | Comment |
|-----------|------------|-----------------------------------|---------|
| Plug      | `OUT`      | `adapter::types::unidirectional::A2X` | UP/DOWN output providing `E_UP`, `E_DOWN` events and `UP`, `DOWN` boolean data. |
| Socket    | `INPUT`    | `adapter::types::unidirectional::AR` | Analog input value (event `E1`, data `D1`). |
| Socket    | `MI`       | `adapter::types::unidirectional::AR` | Center point (e.g., 0.5 for 50%). |
| Socket    | `DEAD`     | `adapter::types::unidirectional::AR` | Deadband around MI (absolute value). Switch-off points = `MI ± ABS(DEAD)`. |
| Socket    | `HYSTERESIS` | `adapter::types::unidirectional::AR` | Hysteresis value (absolute value). Switch-on points = `MI ± (ABS(DEAD) + ABS(HYSTERESIS))`. |

## Functionality

The block converts a continuous analog input (via the `INPUT` adapter) into discrete two-state output signals. The conversion depends on the analog input value `INPUT.D1`, the center point `MI.D1`, the deadband `DEAD.D1`, and the hysteresis `HYSTERESIS.D1`.

The switching behavior is as follows:

- **Switch-on UP** (inclusive): When the input rises to or above `MI.D1 + ABS(DEAD.D1) + ABS(HYSTERESIS.D1)`, the output transitions to the UP state.
- **Switch-off UP** (strict): When the input falls strictly below `MI.D1 + ABS(DEAD.D1)`, the block leaves the UP state and returns to Neutral.
- **Switch-on DOWN** (inclusive): When the input falls to or below `MI.D1 - ABS(DEAD.D1) - ABS(HYSTERESIS.D1)`, the output transitions to the DOWN state.
- **Switch-off DOWN** (strict): When the input rises strictly above `MI.D1 - ABS(DEAD.D1)`, the block leaves the DOWN state and returns to Neutral.

All transitions are triggered by the `INPUT.E1` event, which indicates that a new analog value is available on `INPUT.D1`.

The algorithms executed in each state set the `UP` and `DOWN` data outputs on the OUT adapter accordingly. The corresponding events `OUT.E_UP` and `OUT.E_DOWN` are emitted on each state change.

If `QI = FALSE`, the block remains in a safe state and sets both `UP` and `DOWN` outputs to `FALSE`, suppressing any digital output signals.

## Technical Features

- **Deadband & Hysteresis Separation**: The switch-off threshold is determined only by the deadband, while the switch-on threshold adds the hysteresis value. This creates a well-defined gap between on and off points, preventing oscillation due to noise.
- **Absolute Value Handling**: The `DEAD` and `HYSTERESIS` values are taken as absolute values internally, simplifying configuration even when negative values are provided.
- **Event-Driven Processing**: All evaluations occur on the `INPUT.E1` event, ensuring deterministic behavior aligned with incoming analog updates.
- **Parameterizable Center Point**: The `MI` adapter allows setting an arbitrary center point (e.g., 50%), making the block suitable for a wide range of scaling applications.
- **Unidirectional Adapter Output**: The output is bundled in a single A2X adapter containing both events (`E_UP`, `E_DOWN`) and data (`UP`, `DOWN`), simplifying wiring in 4diac applications.
- **Initialization and De-initialization**: The block supports explicit initialization via `INIT` and safe shutdown via de-initialization, with `QO` reflecting the qualifier state.

## State Overview

The internal state machine consists of six states:

| State      | Description |
|------------|-------------|
| `START`    | Initial power-up state. |
| `Init`     | Performs initialization; sets `QO := QI` and clears UP/DOWN outputs; emits `INITO`. |
| `Neutral`  | Idle state with no UP/DOWN activity; both outputs are `FALSE`. |
| `UP`       | Active UP state; sets `OUT.UP := TRUE`, `OUT.DOWN := FALSE`; emits `E_UP`. |
| `DOWN`     | Active DOWN state; sets `OUT.UP := FALSE`, `OUT.DOWN := TRUE`; emits `E_DOWN`. |
| `DeInit`   | De-initialization state; clears outputs and sets `QO := FALSE`; emits `INITO`. |

Transitions:

- `START → Init` on `INIT` with `QI = TRUE`.
- `Init → Neutral` on first `INPUT.E1` event.
- `Neutral → UP` when `INPUT.D1 >= MI.D1 + ABS(DEAD.D1) + ABS(HYSTERESIS.D1)`.
- `Neutral → DOWN` when `INPUT.D1 <= MI.D1 - ABS(DEAD.D1) - ABS(HYSTERESIS.D1)`.
- `UP → Neutral` when `INPUT.D1 < MI.D1 + ABS(DEAD.D1)`.
- `DOWN → Neutral` when `INPUT.D1 > MI.D1 - ABS(DEAD.D1)`.
- `Neutral → DeInit` on `INIT` with `QI = FALSE`.
- `DeInit → START` unconditionally.

## Application Scenarios

- **Level Monitoring**: Monitoring tank or silo fill levels, where a sensor provides a continuous analog signal and discrete high/low alarms are required with hysteresis to avoid relay chattering.
- **Temperature Control**: Switching heating or cooling elements based on temperature thresholds with a deadband/hysteresis configuration to prevent rapid cycling.
- **Limit Switch Replacement**: Replacing mechanical limit switches with non-contact analog sensors, using the block to derive clean digital position signals.
- **Motor Control**: Generating direction commands (UP/DOWN) for actuators (e.g., valves, drives) based on a setpoint and allowable deadband.
- **Industrial Automation**: In PLC and distributed control systems, where reliable two-state signals must be derived from analog process variables.

## Comparison with Similar Blocks

Compared to a simple comparator or single-threshold hysteresis block, **DualHysteresis_AR_A2X** offers:

- **Two-way (bidirectional) evaluation**: Both rising and falling input signals are processed symmetrically, with separate switch-on and switch-off thresholds.
- **Independent deadband and hysteresis parameters**: The user can define the deadband (non-switching zone) and hysteresis (influence on switch-on point) separately, providing greater flexibility than blocks with a single parameter.
- **Unified output adapter**: All output signals (UP/DOWN events and data) are packaged in one adapter, reducing wiring complexity in comparison to blocks with multiple separate output ports.
- **Safe-state behavior**: When `QI` is false, the block guarantees both outputs are reset to `FALSE`, which is not always the case in simpler hysteresis implementations.

## Conclusion

The **DualHysteresis_AR_A2X** function block provides a robust and flexible solution for converting analog signals into discrete UP/DOWN control outputs. By separating deadband and hysteresis, it offers precise control over switching behavior, preventing oscillation and ensuring reliable operation in industrial automation environments. Its event-driven design, adapter-based interfaces, and clear state machine make it well-suited for integration into 4diac-based control applications.