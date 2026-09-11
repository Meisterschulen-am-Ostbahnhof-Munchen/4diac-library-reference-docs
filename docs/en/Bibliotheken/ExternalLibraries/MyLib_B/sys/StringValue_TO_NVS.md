# StringValue_TO_NVS


![StringValue_TO_NVS_network](./StringValue_TO_NVS_network.svg)

![StringValue_TO_NVS](./StringValue_TO_NVS.svg)

* * * * * * * * * *

## Introduction

The **StringValue_TO_NVS** subapplication provides a generic solution for reading a String-VT value and persisting it into the non-volatile storage (NVS) of an ESP32. It combines the `isobus::UT::io::StringValue::StringValue_IS` function block for input acquisition with the `logiBUS::storage::esp32_nvs::NVS` function block for durable storage. The subapplication also exposes the stored value on its output interface. It was extracted from a larger exercise project to be reused as a standalone, generic component.

## Interface Structure

### **Event Inputs**

The subapplication does not expose any event inputs. Initialization and data flow are handled internally.

### **Event Outputs**

- **IND** (Event): Emitted after a read or write operation completes, indicating that a new value has been stored or retrieved.

### **Data Inputs**

- **KEY** (STRING): The key name under which the value is stored in the NVS.
- **u16ObjId** (UINT): The object identifier for the String-VT value. Defaults to `ID_NULL`.

### **Data Outputs**

- **VALUEO** (STRING): The current string value that has been read from the NVS or acquired from the input.

### **Adapters**

The subapplication does not expose any adapter interfaces.

## Functionality

The subapplication operates as follows:

1. **Initialization**: On startup, the internal `NVS` function block performs its initialization. The resulting `INITO` event is immediately routed to the `GET` event of the same block, causing the currently stored value to be read from the NVS.
2. **Value acquisition**: The `StringValue_IS` function block acquires a String-VT value based on the provided `u16ObjId`. When a new value is available, its `IND` output event triggers the `NVS.SET` event.
3. **Storage**: The acquired string value (from `StringValue_IS.IN`) is written to the NVS under the configured `KEY`. After a successful write operation (`NVS.SETO`), the subapplication emits the `IND` output event to signal completion.
4. **Read/Query**: The `GETO` event from the `NVS` block is forwarded to the `Q_StringValue` query function block, which converts the internal representation (`pau8String`) into a proper String-VT format. Simultaneously, `GETO` is also routed to the subapplication's `IND` output.
5. **Output**: The `VALUEO` output continuously reflects the value stored in the NVS. It is updated after each `SET` or `GET` operation.

## Technical Features

- **Generic NVS key**: The storage key is configurable via the `KEY` input.
- **Object ID binding**: The `u16ObjId` input selects the correct String-VT object in the ISOBUS context.
- **Automatic initialization**: On startup, the NVS block is initialized and the stored value is read automatically without external triggering.
- **Default value handling**: The NVS block is configured with an empty default value (`STRING#''`) for the first-time retrieval.
- **Event acknowledgment**: The `IND` event output signals successful read or write operations.
- **Internal wiring encapsulated**: All event and data connections between the internal function blocks are hidden behind the subapplication interface, simplifying reuse.

## State Overview

The internal state machine is primarily managed by the `NVS` function block from the `logiBUS` library. The typical states are:

1. **Initialization (INIT)**: The NVS block initializes the underlying storage backend.
2. **Get (GET)**: Reads the value from the NVS and makes it available on `VALUEO`.
3. **Set (SET)**: Writes the input value to the NVS under the configured key.

The internal event paths are:

- `NVS.INITO → NVS.GET` (auto-read stored value after initialization)
- `StringValue_IS.IND → NVS.SET` (store a newly acquired value)
- `NVS.GETO → Q_StringValue.REQ` and `NVS.GETO → IND` (query and indication)

## Application Scenarios

- **IoT device configuration**: Storing sensor values, device names, or user-adjustable parameters that must survive a reboot or power loss.
- **ISOBUS integration**: Working with ISO 11783 String-VT objects to persist display strings, diagnostic messages, or configuration data in agricultural automation systems.
- **Generic reuse**: Because both the key and object ID are parameterizable, the subapplication can be instantiated multiple times within a project for storing different values.
- **Parameter snapshotting**: Saving runtime states or calibration values for later retrieval during development or commissioning.

## Comparison with Similar Blocks

| Feature | StringValue_TO_NVS | NVS (storage only) | StringValue_IS (input only) |
|---------|--------------------|--------------------|------------------------------|
| Reads a String-VT value | ✅ | ❌ | ✅ |
| Stores value in NVS | ✅ | ✅ | ❌ |
| Exposes stored value as output | ✅ | ✅ | ❌ |
| Handles initialization automatically | ✅ | ✅ (via INIT) | ❌ |
| Provides query response via Q_StringValue | ✅ | ❌ | ❌ |
| Encapsulates event wiring | ✅ | ❌ | ❌ |

Compared to using the `NVS` and `StringValue_IS` function blocks separately, this subapplication bundles the necessary event wiring, initialization sequence, and query logic into a single reusable component, reducing integration effort and the risk of wiring errors.

## Conclusion

The `StringValue_TO_NVS` subapplication is a well-structured, generic solution for reading a String-VT value and persisting it in the ESP32 NVS. It simplifies project integration by encapsulating initialization, storage, and query behavior behind a clean interface. The exposed `IND` event and `VALUEO` output make it straightforward to incorporate into larger IEC 61499 applications, and its parameterizable inputs provide the flexibility needed for reuse across different contexts.
