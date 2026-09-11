# NumericValue_PHYS_TO_INI


![NumericValue_PHYS_TO_INI_network](./NumericValue_PHYS_TO_INI_network.svg)

![NumericValue_PHYS_TO_INI](./NumericValue_PHYS_TO_INI.svg)

* * * * * * * * * *

## Introduction

The `NumericValue_PHYS_TO_INI` subapplication provides a generic, reusable mechanism for reading a scaled numeric process or device value (the PHYS variant) and persistently storing it in an INI storage. It combines a physical-value acquisition block, an INI storage block, and a query-conversion block to deliver a complete read-and-persist chain. The entity is designed to be parameterized via a key, a section name, and a numeric object-pool descriptor that carries scaling, offset, and decimal-place information.

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

| Name | Type | Description |
|------|------|-------------|
| `IND` | Event | Indicates that a value has been successfully stored in or retrieved from the INI storage. |

### **Data Inputs**

| Name | Type | Description |
|------|------|-------------|
| `KEY` | STRING | Key name under which the value is stored in the INI file. |
| `SECTION` | STRING | Section name within the INI file used for grouping the key. |
| `stObj` | `logiBUS::utils::conversion::phys::NumericObjectPool_S` | Object-pool descriptor containing the numeric object ID, scale factor, offset, and number of decimals. Initial value: `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 0)`. |

### **Data Outputs**

| Name | Type | Description |
|------|------|-------------|
| `VALUEO` | REAL | The current value that has been stored or retrieved from the INI storage. |

### **Adapters**

None.

## Functionality

The subapplication orchestrates three internal function blocks:

- `NumericValue_PHYS` – acquires a raw process value and converts it into a scaled physical `REAL` value using the parameters contained in `stObj`.
- `INI` – stores and retrieves that `REAL` value in an INI file, addressed by `KEY` and `SECTION`.
- `Q_NumericValue_PHYS` – a query function that reconstructs the physical representation from the stored numeric value, using the same object-pool descriptor.

The operational sequence is as follows:

1. Upon initialization, the `INI` block performs an automatic `GET` operation (`INITO → GET`) to load any previously persisted value for the given key and section.
2. When a new value is available, `NumericValue_PHYS` emits an `IND` event, triggering the `INI.SET` operation. The converted physical value (`rPhys`) is written to the INI storage.
3. Both the completion of a `SET` and the completion of a `GET` operation propagate an `IND` event to the subapplication output.
4. On every `GETO`, the retrieved value is forwarded to the query block `Q_NumericValue_PHYS` for further conversion, and simultaneously made available as `VALUEO`.

The internal data connections ensure that the same `stObj` descriptor is used consistently by both the acquisition block (for scaling while reading) and the query block (for reverse scaling), while `KEY` and `SECTION` only reference the INI storage.

## Technical Features

- **Generic parameterization** – The subapplication does not hard-code any key, section, or object ID. All addressing and scaling information is supplied through the data inputs.
- **Scaling support** – The `stObj` descriptor allows arbitrary `REAL` scaling (`r32Scale`, `i32Offset`) and decimal rounding (`u8Decimals`), making the block suitable for engineering-unit conversions.
- **Non-volatile persistence** – Values are stored in an INI file, surviving restarts of the runtime environment.
- **Default initial value** – If no stored value exists, the `INI` block falls back to a default value of `REAL#0.0`.
- **Automatic init-read** – On start-up the stored value is fetched automatically, so the `VALUEO` output reflects the persisted state without an explicit external trigger.

## State Overview

The internal `INI` function block follows this state transition pattern:

- **Uninitialized** – After power-up or before `INIT` is accepted.
- **Initializing** – On receiving the `INI.INITO` event, the block immediately issues a `GET` operation to load the stored value.
- **Operational (Set/Get)** – In this state, the block alternates between `SET` (triggered by `NumericValue_PHYS.IND`) and `GET` operations, each producing a corresponding `SETO` or `GETO` confirmation event.
- **Fault** – If a storage access fails, the `INI` block raises an error condition; the error handling is external to this subapplication and must be monitored via the standard `QO`/`EO` signals of the INI block (not exposed at this level).

The `NumericValue_PHYS` acquisition block operates independently and is enabled by its constant `QI := TRUE`; it continuously acquires and converts values as they arrive.

## Application Scenarios

- **Machine parameter persistence** – Storing calibrated sensor values or scaled setpoints in an INI configuration file so they remain valid after a controller restart.
- **Data logging with scaling** – Saving scaled engineering values (e.g., temperatures, pressures, flow rates) in a compact textual format, where the original unit conversion is defined by the object-pool descriptor.
- **System configuration exchange** – Using a standardized INI section/key structure to export/import process values between different plant components or HMI layers.
- **Generic library reuse** – Because the subapplication is fully generic, it can be instantiated multiple times within a larger application, each instance configured with its own key, section, and scaling descriptor, without code changes.

## Comparison with Similar Blocks

| Aspect | `NumericValue_PHYS_TO_INI` | `NumericValue` (raw, no storage) | `NumericValue_TO_INI` (non-phys variant) |
|--------|----------------------------|-----------------------------------|------------------------------------------|
| Value scaling | Yes – through `stObj` (scale, offset, decimals) | No – raw value only | No – raw value only |
| Persistence in INI | Yes | No | Yes |
| Automatic init-read | Yes | N/A | Yes |
| Required external data | `KEY`, `SECTION`, `stObj` | None (only acquisition parameters) | `KEY`, `SECTION` |
| Output behavior | `VALUEO` is the stored/retrieved value | Output is the live raw value | Output is the stored raw value |

The distinctive advantage of this subapplication is the combination of scaling and persistence in a single reusable component, which reduces wiring effort and ensures consistent conversion parameters between acquisition and storage.

## Conclusion

`NumericValue_PHYS_TO_INI` is a compact and generic subapplication that solves the recurring task of reading a scaled physical process value and making it persistable in an INI storage. Its well-defined interface, automatic initialization behavior, and built-in scaling support make it a suitable building block for larger automation projects that require configuration persistence with engineering-unit correctness. The block is particularly valuable in environments where multiple measured values must be archived or restored across restarts without redundant application logic.
