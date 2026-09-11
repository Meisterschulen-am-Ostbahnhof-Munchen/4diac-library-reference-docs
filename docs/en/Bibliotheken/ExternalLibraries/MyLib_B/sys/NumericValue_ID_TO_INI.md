# NumericValue_ID_TO_INI


![NumericValue_ID_TO_INI_network](./NumericValue_ID_TO_INI_network.svg)

![NumericValue_ID_TO_INI](./NumericValue_ID_TO_INI.svg)

* * * * * * * * * *
## Introduction

The **NumericValue_ID_TO_INI** subapplication is a generic, reusable component that reads a numeric value from a Virtual Terminal (VT) using an ID-based variant and stores it persistently in an INI file. It combines the ISOBUS UT (Universal Terminal) NumericValue interface with IEC 61131 conversion functions and the Eclipse 4diac INI storage service. The subapplication is designed for scenarios where a numeric VT value, identified by a unique object ID, needs to be captured and saved to a configuration or data file, while also being queryable for further processing.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
|------|------|-------------|
| *(none)* | — | This subapp has no event inputs. All activity is triggered internally via initialization or via the internal NumericValue FB indication. |

### **Event Outputs**

| Name | Type | Description |
|------|------|-------------|
| `IND` | Event | Indicates that a read, write, or initialization operation has completed. Emitted either after a successful INI SET operation, after a GET operation, or after querying the VT value. |

### **Data Inputs**

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `KEY` | `STRING` | — | The key (name) under which the numeric value is stored in the INI file. |
| `SECTION` | `STRING` | — | The section of the INI file where the key is located. |
| `u16ObjId` | `UINT` | `ID_NULL` | The object ID of the numeric VT value to be read (source identifier for the VT communication). |

### **Data Outputs**

| Name | Type | Description |
|------|------|-------------|
| `VALUEO` | `UDINT` | The numeric value read from the VT and stored/retrieved from the INI file. This output is updated after a GET or SET operation. |

### **Adapters**

None.

## Functionality

The subapp provides a complete data path from a Virtual Terminal numeric object to persistent INI storage:

1. **VT Read**: The internal `NumericValue_ID` FB (`isobus::UT::io::NumericValue::NumericValue_ID`) reads the numeric value from the VT based on the provided `u16ObjId`. When the VT delivers the value, the FB raises its `IND` output event.

2. **Conversion**: The raw `DWORD` value from the VT is passed to the `F_DWORD_TO_UDINT` conversion FB, which converts it to a `UDINT` data type suitable for storage and further processing.

3. **Persistent Storage**: The converted value is written to the INI file via the `INI` FB (`eclipse4diac::storage::INI`). The parameters `KEY` and `SECTION` define the exact location in the file where the value is stored. After successful storage, the `SETO` event is emitted, which triggers the subapp's `IND` output.

4. **Initialization / Read-back**: Upon initialization, the INI FB performs a `GET` operation (via the internal connection from `INITO` to `GET`) to retrieve the previously stored value. After the GET completes, two actions occur:
   - The `GETO` event triggers the `Q_NumericValue` FB, which is used to query or set the VT numeric value with the retrieved stored value (ensuring synchronization).
   - The `GETO` event also propagates directly to the subapp's `IND` output, informing the external application that the read-back is complete.

5. **Output**: The retrieved value from the INI FB (`VALUEO`) is provided on the subapp's `VALUEO` output and simultaneously sent to the `Q_NumericValue` FB for VT synchronization.

## Technical Features

- **Generic design**: The subapp is fully reusable and not bound to a specific application context – the `KEY`, `SECTION`, and `u16ObjId` parameters make it flexible for any numeric VT object.
- **DWORD → UDINT conversion**: Ensures type compatibility between the ISOBUS layer (which delivers `DWORD`) and the storage layer (which expects `UDINT`).
- **Self-initializing**: On startup, the INI FB automatically loads the last stored value via the internal `INITO → GET` connection.
- **Two-way synchronization**: After reading from the VT, the value is stored to INI; after reading from INI, the value is pushed to the VT via `Q_NumericValue`.
- **Single event output** (`IND`): Allows the calling application to react to any of the internal operations (SET, GET, or query) in a unified manner.
- **Direct output visibility**: The current value is always available on `VALUEO`, allowing external monitoring or further processing without additional event handling.
- **Use of standard libraries**: Relies on established ISOBUS UT, IEC 61131 conversion, and Eclipse 4diac storage functions, ensuring compatibility and maintainability.

## State Overview

The subapp operates in a sequential, event-driven manner without explicit state variables. The internal event flow can be summarized as:

1. **Idle**: The subapp waits for an internal trigger (either a VT indication or initialization).
2. **VT Read State**: The `NumericValue_ID` FB receives a new value from the VT. Upon indication (`IND`), the value is forwarded to the conversion FB.
3. **Conversion State**: `F_DWORD_TO_UDINT` processes the value and issues a confirmation (`CNF`).
4. **Storage State**: The `INI` FB writes the converted value using `KEY`/`SECTION`. After the write completes, `SETO` is raised, and the subapp emits `IND`.
5. **Initialization / Read-back State**: On startup, the `INI` FB automatically issues a `GET`. When the value is retrieved (`GETO`), the `Q_NumericValue` FB is triggered to synchronize the VT, and `IND` is emitted simultaneously.

The use of implicit event chaining (via internal connections) ensures deterministic behavior without race conditions.

## Application Scenarios

- **VT parameter persistence**: Storing operator-set numeric parameters (e.g., setpoints, machine limits) from an ISOBUS Virtual Terminal to a local INI file, so that values survive a power cycle.
- **Automated machine configuration**: Reading a numeric value from a VT object and writing it to a configuration file used by other automation components.
- **Data logging / audit trails**: Capturing numeric VT values (e.g., speed, temperature) and storing them in INI-based logs for later analysis.
- **System synchronization**: Using the INI file as a bridge between a VT and a control application – the subapp ensures both ends are updated consistently.
- **Reusable building block**: Since it was extracted from an existing application for reuse, it can be embedded in multiple FB networks without modification.

## Comparison with Similar Blocks

| Feature | NumericValue_ID_TO_INI | Direct NumericValue FB (e.g., `NumericValue_ID`) | INI Storage FB (standalone) |
|---------|------------------------|-------------------------------------------------|-----------------------------|
| **Reads from VT** | Yes | Yes | No |
| **Writes to INI** | Yes | No | Yes |
| **Type conversion** | Automatic (DWORD→UDINT) | Not included | Not included |
| **Initialization load** | Yes (automatic GET) | No | No |
| **VT synchronization after read** | Yes (via `Q_NumericValue`) | No | No |
| **Encapsulation** | High – single subapp | Low – requires external wiring | Low – requires external wiring |
| **Reusability** | High (generic parameters) | Medium (requires setup) | Medium (requires setup) |
| **Complexity** | Moderate (internal logic hidden) | Low but manual effort needed | Low but manual effort needed |

The main advantage of this subapp over using the individual blocks separately is the **complete integration** – the user does not have to wire the event/data connections, handle the initialization sequence, or manage the conversion logic. It provides a ready-to-use, self-contained solution.

## Conclusion

The **NumericValue_ID_TO_INI** subapplication is a well-structured, generic component that bridges the gap between ISOBUS Virtual Terminal numeric values and persistent INI storage. It encapsulates a complete data flow – from VT reading, through type conversion, to file storage and back-synchronization – within a single, easy-to-use interface. With only three input parameters and one event output, it is both simple to integrate and highly reusable. The automatic initialization behavior and the implicit synchronization with the VT make it particularly suitable for applications requiring persistent, consistent numeric parameters. Its design follows established standards (IEC 61499-2, IEC 61131) and leverages proven library components, ensuring reliability and maintainability in industrial automation environments.