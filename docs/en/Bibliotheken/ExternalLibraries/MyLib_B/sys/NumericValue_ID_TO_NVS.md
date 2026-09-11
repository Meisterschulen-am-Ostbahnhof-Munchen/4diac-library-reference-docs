# NumericValue_ID_TO_NVS


![NumericValue_ID_TO_NVS_network](./NumericValue_ID_TO_NVS_network.svg)

![NumericValue_ID_TO_NVS](./NumericValue_ID_TO_NVS.svg)

* * * * * * * * * *
## Introduction

NumericValue_ID_TO_NVS is a generic IEC 61499 subapplication that reads a numeric value identified by an object ID and stores or retrieves that value in the non-volatile storage (NVS) of an ESP32. It combines an ISOBUS-compatible numeric value input block, a data type conversion function block, and an NVS storage block into a reusable component. The subapplication exposes a simple interface with one event output, two data inputs, and one data output.

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

| Event   | Comment                                                                 |
|---------|-------------------------------------------------------------------------|
| `IND`   | Indicates that either a SET or GET operation on the NVS has been completed. |

### **Data Inputs**

| Name      | Type     | Initial Value | Comment                            |
|-----------|----------|---------------|------------------------------------|
| `KEY`     | `STRING` | —             | Key name used in the NVS storage.  |
| `u16ObjId`| `UINT`   | `ID_NULL`     | Object ID of the numeric value to be processed. |

### **Data Outputs**

| Name      | Type     | Comment                                            |
|-----------|----------|-----------------------------------------------------|
| `VALUEO`  | `UDINT`  | Numeric value that is currently stored in the NVS.  |

### **Adapters**

None.

## Functionality

The subapplication internally uses three main function blocks:

1. `NumericValue_ID`  
   Reads a numeric value associated with the input `u16ObjId`. Its event output `IND` triggers the following processing steps.

2. `F_DWORD_TO_UDINT`  
   Converts the incoming numeric value from `DWORD` to `UDINT`. This ensures that the value is stored in the NVS in a consistent unsigned double integer format.

3. `NVS`  
   Stores the converted value under the key provided by `KEY` and makes it available again on the output `VALUEO`.

The internal event flow works as follows:

- When the `NumericValue_ID` block produces an `IND` event, the conversion block is triggered via `REQ`.
- After conversion, the `CNF` event triggers the NVS block to perform a `SET` operation.
- A completed SET operation is reported on `SETO`, which is propagated to the subapplication output `IND`.
- On initialization, the NVS block performs a GET operation automatically after its `INITO` event. The retrieved value is then assigned to `VALUEO` and also passed to the internal `Q_NumericValue` block.

## Technical Features

- Generic NVS key handling through the `KEY` input.
- Object-ID-based value selection through `u16ObjId`.
- Automatic conversion from `DWORD` to `UDINT` before storage.
- NVS default value configured as `UDINT#0`.
- Initial object ID default is `ID_NULL`, imported from `isobus::UT::Q::const::IDs`.
- All internal event sequencing is self-contained; no external event input is required.
- Provides a simple completion signal via the `IND` event output.
- This subapplication is suitable for reuse in larger ISOBUS or ESP32-based systems.

## State Overview

The subapplication can be described by the following internal operating states:

1. **Initialization State**  
   After startup, the NVS block initializes. Its `INITO` event automatically triggers a GET request.

2. **GET State**  
   The NVS reads the value associated with `KEY`. When completed, the retrieved value is written to `VALUEO` and the `IND` event is emitted.

3. **Value Update State**  
   When a new numeric value is read from the `NumericValue_ID` block, it is converted and then stored in the NVS via a SET operation.

4. **Completion State**  
   After the SET operation finishes, the `SETO` event propagates to the external world through `IND`.

## Application Scenarios

- Storing ISOBUS numeric implement values in ESP32 non-volatile memory.
- Persisting calibration values or configuration parameters that are addressed by object IDs.
- Building reusable components for V3 / ISOBUS object pool applications.
- Replacing direct manual handling of NVS storage with a generic, event-driven storage pattern.

## Comparison with Similar Blocks

- Compared to a straightforward connection between `NumericValue_ID` and `NVS`, this subapplication encapsulates the required data conversion and event handling, reducing wiring complexity.
- Compared to non-ID numeric storage blocks, this variant selects values by `u16ObjId`, making it suitable for ISOBUS-related data.
- Compared to blocks that only read or only write, this subapplication supports both reading and writing while exposing a single completion event.

## Conclusion

NumericValue_ID_TO_NVS is a compact and reusable subapplication for reading numeric values by object ID and persisting them in ESP32 NVS storage. It simplifies integration by hiding conversion, initialization, and event sequencing behind a small interface. It is especially useful in ISOBUS-based systems where numeric values need to be preserved across power cycles.