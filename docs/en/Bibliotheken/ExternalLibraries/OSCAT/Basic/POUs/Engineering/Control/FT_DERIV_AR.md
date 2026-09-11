# FT_DERIV_AR

![FT_DERIV_AR](./FT_DERIV_AR.svg)

* * * * * * * * * *

## Introduction

`FT_DERIV_AR` is an adapter wrapper around the OSCAT function block `OSCAT::Basic::POUs::Engineering::Control::FT_DERIV`. It provides a clean adapter-based interface for the derivative calculation so that the block can be used in subapplications that communicate through unidirectional `AR` / `AUDI` adapters.

The input signal is received through an `AR_IN` socket. The calculated derivative and the internal intermediate values are exposed through three adapter plugs. The configuration values `K` and `run` are kept as plain input variables so that they can be set as parameters when the block is instantiated.

The wrapper also solves a library-coupling problem: the generic `adapter` library and the `OSCAT` library can remain independent, while the bridge itself is encapsulated in a dedicated wrapper type.

## Interface Structure

### **Event Inputs**

| Event | Type | Description |
|-------|------|-------------|
| `INIT` | `EInit` | Initialization request; passed through to `FT_DERIV.EINIT`. |
| `RST` | `Event` | Resets the derivative history; passed through to `FT_DERIV.RST`. |

### **Event Outputs**

| Event | Type | Description |
|-------|------|-------------|
| `INITO` | `EInit` | Initialization confirmation; passed back from `FT_DERIV.INITO`. |

### **Data Inputs**

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `K` | `REAL` | `1.0` | Derivation factor; passed through to `FT_DERIV.K`. |
| `run` | `BOOL` | `TRUE` | Calculation enable; passed through to `FT_DERIV.run`. |

### **Data Outputs**

This block has no plain data outputs. All calculation results are exposed through adapter plugs.

### **Adapters**

| Kind | Name | Adapter Type | Description |
|------|------|--------------|-------------|
| Socket | `AR_IN` | `adapter::types::unidirectional::AR` | REAL input signal; `E1` triggers the calculation and `D1` carries the input value. |
| Plug | `AR_OUT` | `adapter::types::unidirectional::AR` | Calculated derivative (`out`, REAL). |
| Plug | `AR_DELTA_IN` | `adapter::types::unidirectional::AR` | Difference of the input signal (`delta_in`, REAL). |
| Plug | `AUDI_DELTA_T` | `adapter::types::unidirectional::AUDI` | Time difference in microseconds (`delta_t`, UDINT). |

## Functionality

The internal network consists of the OSCAT FB `FT_DERIV` and a three-channel D flip-flop `E_D_FF_ANY_3`. The wrapper orchestrates the event and data flow as follows:

1. **Initialization** – `INIT` is forwarded to `FT_DERIV.EINIT`. The internal block performs its initialization and returns `INITO`. If `INIT` is left unconnected, the `EInit` event fires automatically once at deployment.
2. **Input request** – When the `AR_IN` adapter receives an event (`AR_IN.E1`), the corresponding REAL value (`AR_IN.D1`) is written to `FT_DERIV.in`, and `FT_DERIV.REQ` is triggered.
3. **Derivative calculation** – The internal `FT_DERIV` calculates the derivative using the current input, the configured factor `K`, and the enable flag `run`.
4. **Output latching** – When `FT_DERIV.CNF` is emitted, the internal `E_D_FF_ANY_3` latches `out`, `delta_t`, and `delta_in` into its three channels. The flip-flop then emits its output event `EO`, which is routed to all three plugs: `AR_OUT.E1`, `AR_DELTA_IN.E1`, and `AUDI_DELTA_T.E1`. The corresponding `D1` values are taken from the latched `Q1`, `Q2`, and `Q3` outputs.
5. **Reset** – `RST` is passed through to `FT_DERIV.RST`, clearing the internal derivative history.

Because all three values are latched from the same `CNF` event, downstream consumers receive a consistent snapshot of the calculation cycle.

## Technical Features

- **Adapter-based result boundary** – The three calculation results are available only through `AR` / `AUDI` plugs; no plain data output is needed.
- **Composite implementation** – Contains `OSCAT::Basic::POUs::Engineering::Control::FT_DERIV` and `iec61499::events::E_D_FF_ANY_3`.
- **Simultaneous output update** – The internal three-channel D flip-flop ensures that `AR_OUT`, `AR_DELTA_IN`, and `AUDI_DELTA_T` are updated together.
- **Decoupled library design** – The wrapper is placed in a dedicated library project that depends on both `adapter` and `OSCAT`, avoiding a direct dependency between the two base libraries.
- **Pass-through lifecycle/reset** – `INIT`, `INITO`, and `RST` behave exactly like the underlying `FT_DERIV` interface.
- **Configuration inputs** – `K` and `run` are plain `InputVars` with default values `1.0` and `TRUE`.
- **Adapter-based wiring** – The block can be connected to other `AR` / `AUDI` blocks using ordinary `AdapterConnections`; no raw `.E1` / `.D1` access from a parent network is needed.

## State Overview

`FT_DERIV_AR` is a composite FB type, so it does not define an explicit ECC state diagram. Its runtime behavior can be described by the following phases:

| Phase | Description |
|-------|-------------|
| Initialization | `INIT` is forwarded to `FT_DERIV.EINIT`; `INITO` confirms completion. |
| Ready / Waiting | The block is idle and waits for an `AR_IN.E1` event. |
| Calculating | `FT_DERIV.REQ` is active; the internal derivative calculation has not yet completed. |
| Output Update | `FT_DERIV.CNF` arrives and the flip-flop latches/emits `out`, `delta_t`, and `delta_in` to the adapter plugs. |
| Reset | `RST` clears the internal input/time history stored in `FT_DERIV`. |

When `run` is `FALSE`, the internal calculation is disabled and the output plugs are not updated with a new derivative.

## Application Scenarios

- **Frequency / rate measurement** – If the input `AR_IN` receives a pulse counter or incremental counter value, `AR_OUT` delivers the derivative directly in Hz when `K = 1.0`.
- **Rate-of-change monitoring** – The block can monitor the velocity of a process value such as pressure, temperature, or fill level and make the result available to adapter-based alarm or supervision blocks.
- **Adapter-based subapplications** – In compositions that already use `AR` / `AUDI` adapters, `FT_DERIV_AR` can be integrated without falling back to plain event/data connections.
- **Plain value conversion** – If a downstream block requires a plain `REAL` value instead of an adapter, the `AR_OUT` plug can be connected to a generic conversion block such as `adapter::conversion::unidirectional::AR_R_TO_REAL`.
- **Batch or mode changes** – `RST` can be used to clear the derivative history when the process is restarted or when a new operating mode begins.

## Comparison with Similar Blocks

| Block | Interface | Outputs | Typical Use |
|-------|-----------|---------|-------------|
| `FT_DERIV` | Plain events/data, no adapters | `out`, `delta_t`, `delta_in` | OSCAT-native networks where adapter boundaries are not required |
| `FT_DERIV_AR` | Adapter-based result boundary | `AR_OUT`, `AR_DELTA_IN`, `AUDI_DELTA_T` | Adapter-oriented 4diac subapplications and library decoupling |

`FT_DERIV_AR` does not provide a new differentiation algorithm; it is a connectivity wrapper around the `FT_DERIV` calculation. Compared with using `FT_DERIV` directly, it adds adapter compatibility, output latching, and a clean separation between the `adapter` and `OSCAT` libraries.

## Conclusion

`FT_DERIV_AR` is a practical adapter wrapper for the OSCAT `FT_DERIV` function block. It preserves the original calculation semantics while offering an adapter-friendly interface, consistent output latching, and clean library decoupling. This makes it well suited for modern 4diac subapplication designs that rely on unidirectional `AR` / `AUDI` adapters and want to avoid mixing raw `.E1` / `.D1` access or plain data pins across library boundaries.
