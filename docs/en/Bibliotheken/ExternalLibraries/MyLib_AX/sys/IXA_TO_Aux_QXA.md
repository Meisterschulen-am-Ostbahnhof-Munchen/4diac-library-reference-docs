# IXA_TO_Aux_QXA


![IXA_TO_Aux_QXA_network](./IXA_TO_Aux_QXA_network.svg)

![IXA_TO_Aux_QXA](./IXA_TO_Aux_QXA.svg)

* * * * * * * * * *

## Introduction

The **IXA_TO_Aux_QXA** subapplication serves as a generic, adapter-based bridge between a logiBUS digital input channel and an ISOBUS auxiliary output (Aux_QXA). It is designed for reuse in scenarios where a physical input from a logiBUS I/O system must be mapped to a specific auxiliary output position on an ISOBUS network, commonly found in tractor-implement control systems.

By encapsulating the internal conversion logic (from the logiBUS domain to the ISOBUS domain), the subapp provides a clean, reusable interface with only two data inputs, hiding all protocol-specific details from the surrounding logic.

## Interface Structure

### **Event Inputs**

The subapp has no event inputs. All data flow is purely continuous (non-event-triggered).

### **Event Outputs**

The subapp has no event outputs.

### **Data Inputs**

| Name      | Type                              | Initial Value | Description                                                                                       |
|-----------|-----------------------------------|---------------|---------------------------------------------------------------------------------------------------|
| `Input`   | `logiBUS::io::DI::logiBUS_DI_S`   | `Invalid`     | Identifies the physical logiBUS digital input (e.g., `Input_I1` .. `Input_I8`) to be read.        |
| `iInpNr`  | `USINT`                           | `0`           | Number of the auxiliary array entry; corresponds to the position in the pool. The first aux input in the pool is `iInpNr = 0`, the second is `1`, etc. |

### **Data Outputs**

The subapp has no data outputs. Results are propagated internally via adapter connections.

### **Adapters**

The subapp does not expose adapters at its interface. Internally, an adapter connection links the logiBUS input block to the ISOBUS auxiliary output block.

## Functionality

The subapp performs the following tasks:

1. Receives a digital input identifier (`Input`) of type `logiBUS_DI_S` and an auxiliary index (`iInpNr`).
2. Internally instantiates a `logiBUS_IXA` function block, which reads and processes the specified logiBUS digital input. This block is enabled via the `QI` parameter set to `TRUE`, and its `PARAMS` parameter is deliberately left empty (visible attribute hidden).
3. The processed input signal is forwarded through an adapter connection (`IX.IN` → `QX.OUT`) to an instantiated `Aux_QXA` function block from the ISOBUS utility library.
4. The `Aux_QXA` block maps the incoming adapter data to the auxiliary output position specified by `iInpNr`, thereby activating the corresponding ISOBUS auxiliary function.

The combination of both internal FBs with `QI = TRUE` ensures that the entire chain is active as soon as valid input data is available.

## Technical Features

- **Adapter-based coupling**: The connection between the logiBUS input handling and the ISOBUS output handling is realized via a typed adapter connection, ensuring clean, protocol-compliant data transfer.
- **Generic input selection**: The `Input` parameter allows selecting any of the physical inputs `I1`..`I8`, making the subapp reusable for different wiring configurations without modification.
- **Pool-position mapping**: The `iInpNr` parameter maps the selected input to a dedicated position in the ISOBUS auxiliary output pool.
- **Hidden internal parameters**: The `PARAMS` parameter of the `logiBUS_IXA` block is hidden in the graphical representation (`Visible = false`), reducing clutter in the network editor.
- **Default-enabled operation**: Both internal function blocks are pre-enabled with `QI = TRUE`, requiring no additional start-up sequencing.
- **Standard-conformant types**: Uses types defined according to IEC 61499-2, with the overall subapp packaged for reuse in different projects.

## State Overview

The subapp does not implement an explicit state machine. Its operational behavior is derived from the states of the internal function blocks:

- **Idle / invalid input**: If `Input` is set to `Invalid` (as per its initial value), the `logiBUS_IXA` block will not deliver valid data, and the auxiliary output remains inactive.
- **Active input**: When a valid input identifier (`Input_I1`..`Input_I8`) is applied, the `logiBUS_IXA` block processes the signal and forwards it via the adapter to `Aux_QXA`.
- **Enabled output**: With `QI = TRUE` on `Aux_QXA` and a valid adapter data stream, the auxiliary output position (`iInpNr`) is continuously updated to reflect the state of the selected logiBUS input.

Users should ensure that the `Input` value matches an actual, configured logiBUS input; otherwise, the output remains inactive.

## Application Scenarios

- **Tractor implement control**: Mapping physical push-buttons or switches on an implement (connected via logiBUS) to ISOBUS auxiliary outputs for functions such as raising/lowering implements, activating PTOs, or controlling hydraulic valves.
- **Reusable input-to-output bridge**: Since the subapp is generic, it can be instantiated multiple times in a project — once for each physical input that shall control a distinct auxiliary output.
- **Migration of legacy wiring**: When upgrading from direct-wired controls to ISOBUS-based control, this subapp allows reusing existing logiBUS input modules without changing the wiring.

## Comparison with Similar Blocks

Compared to a direct hard-wired connection between a logiBUS input module and an ISOBUS auxiliary output, this subapp offers:

- **Protocol abstraction**: The user does not need to interact with raw adapter types or internal FB configurations.
- **Reusability**: Extracted from a larger exercise project (previously part of `Uebung_003c_sub_AX`), it can be dropped into any 4diac project with minimal effort.
- **Clarity**: The two-parameter interface (`Input`, `iInpNr`) is far simpler than manually wiring internal FBs and checking adapter compatibility.
- **Limitation**: It provides a one-way signal path (input → output) without bidirectional feedback. Applications requiring status readback from the ISOBUS side would need a different, more elaborate subapp.

## Conclusion

The **IXA_TO_Aux_QXA** subapplication is a compact, generic, and reusable solution for connecting logiBUS digital inputs to ISOBUS auxiliary outputs. By encapsulating the protocol adaptation in an adapter-based design, it simplifies project engineering, promotes reuse, and keeps the network representation clean. It is particularly suited for agricultural and off-highway applications built on the 4diac framework.
