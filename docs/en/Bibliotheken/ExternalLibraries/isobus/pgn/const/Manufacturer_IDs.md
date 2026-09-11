# Manufacturer_IDs

![Manufacturer_IDs](./Manufacturer_IDs.svg)

* * * * * * * * * *

## Introduction

This document describes the **Manufacturer_IDs** global constant set from the ISOBUS protocol library. It defines a comprehensive catalog of numeric identifiers for manufacturers as specified by the ISO 11783 (ISOBUS) standard. These constants are used to unambiguously identify the manufacturer of an electronic control unit (ECU) in parameter group numbers (PGNs) and other ISOBUS messages.

## Interface Structure

Since this is a global constant set, it does not have a traditional function block interface with event or data inputs/outputs. Instead, it exposes a large number of named constants, each of type `UINT` (unsigned 16-bit integer), that can be referenced by name in any IEC 61499 application.

### **Event Inputs**

None – this is a data-only constant set.

### **Event Outputs**

None – no event triggers are generated.

### **Data Inputs**

None – constants are read-only and cannot be altered at runtime.

### **Data Outputs**

Each constant acts as an implicit data output. All constants are of type `UINT` and are accessible by their symbol name (e.g., `M_CATERPILLAR`). The set includes over 1,600 named manufacturers, each with a unique value. A few examples are listed below:

| Constant Name                     | Value | Description                          |
|-----------------------------------|-------|--------------------------------------|
| `M_FOR_EXPERIMENTAL_OR_DEVELOPMENTAL_USE_ONLY` | 0     | Reserved for experimental use.       |
| `M_BENDIX_COMMERCIAL_VEHICLE_SYSTEMS` | 1     | Bendix Commercial Vehicle Systems.   |
| `M_ALLISON_TRANSMISSION`         | 2     | Allison Transmission, Inc.           |
| `M_CATERPILLAR`                  | 8     | Caterpillar Inc.                     |
| `M_DEERE_AND_COMPANY_PRECISION_FARMING` | 12 | Deere & Company, Precision Farming.  |
| `M_AGCO`                         | 102   | AGCO (formerly AGCO GmbH & Co.).     |
| `M_CNH_INDUSTRIAL_N_V`           | 94    | CNH Industrial N.V.                  |
| `M_RAVEN_INDUSTRIES`             | 151   | Raven Industries, Inc.               |
| `M_TRIMBLE`                      | 1856  | Trimble Navigation.                  |

The full list is defined in the XML source and includes entries from international manufacturers across agriculture, construction, marine, and automotive sectors.

### **Adapters**

None – this set does not provide adapter interfaces.

## Functionality

The primary purpose of these constants is to provide a standardized, human-readable way to refer to manufacturer identification codes in ISOBUS applications. When constructing or parsing ISOBUS messages, developers can reference these constants instead of memorizing numeric codes. The constants are defined with the prefix `M_` for easy identification in code.

## Technical Features

- **Data Type:** All constants are of type `UINT` (16-bit unsigned integer), matching the ISO 11783 manufacturer ID field width.
- **Total Count:** Over 1,600 distinct manufacturer entries, including some reserved or unused codes (e.g., `M_UNUSED_64`, `M_UNUSED_76`, `M_UNUSED_228`).
- **Versioning:** The set is versioned; comments next to each constant indicate the date of last update or addition.
- **Global Scope:** As a global constant set, these values are available across all function blocks in an application without explicit wiring.
- **Read-Only:** The constants are declared as `CONSTANT` and cannot be modified during runtime.

## State Overview

This constant set does not represent a state machine or dynamic behavior. It is static and immutable; all values are fixed at compile time. Therefore, no state transitions or runtime states exist.

## Application Scenarios

- **ISOBUS ECU Implementation:** When developing an ECU for agricultural or construction equipment, the manufacturer ID must be set according to the ISO 11783 specification. Using these constants ensures that the correct numeric value is used.
- **Message Filtering:** In a diagnostic tool or bus monitor, manufacturer IDs are used to filter or identify messages from specific ECUs. These constants can be used in comparison logic.
- **Configuration and Setup:** When configuring a virtual terminal or task controller, the manufacturer ID of the implement or tractor can be selected from this list.
- **Emulation and Testing:** For simulation or testing, the constant `M_FOR_EXPERIMENTAL_OR_DEVELOPMENTAL_USE_ONLY` (0) can be used to indicate a non-production device.

## Comparison with Similar Blocks

This constant set is analogous to other global constant sets in the ISOBUS library, such as `PGN_Constants` or `SPN_Constants`. While those sets define parameter group numbers and suspect parameter numbers, `Manufacturer_IDs` focuses exclusively on manufacturer identification. The key difference is that manufacturer IDs are unique to each equipment manufacturer, while PGNs and SPNs are standardized by the ISOBUS protocol itself. This set is often referenced alongside PGN and SPN definitions to construct complete ISOBUS messages.

## Conclusion

The `Manufacturer_IDs` global constant set provides a reliable and comprehensive reference for ISOBUS manufacturer identification. By encapsulating the numeric codes as named constants, it enhances code readability, reduces errors, and simplifies compliance with the ISO 11783 standard. Its use is recommended in any IEC 61499 application that interacts with ISOBUS networks.
