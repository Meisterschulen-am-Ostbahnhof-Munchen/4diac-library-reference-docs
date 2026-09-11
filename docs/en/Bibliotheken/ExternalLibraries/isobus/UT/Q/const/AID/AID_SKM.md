# AID_SKM

![AID_SKM](./AID_SKM.svg)

* * * * * * * * * *

## Introduction

AID_SKM is a global constants block that defines attribute identifiers for the soft key mask object within the ISOBUS virtual terminal. It serves as a central repository for constant values, ensuring consistent referencing and maintainability across the automation system.

## Interface Structure

AID_SKM does not possess conventional function block interfaces such as event or data inputs/outputs, nor does it contain adapters. Instead, it declares global constants that are accessible system-wide.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

The block provides the following global constant, which is treated as a read‑only output value:

| Name              | Type   | Initial Value | Comment                                                      |
|-------------------|--------|---------------|--------------------------------------------------------------|
| BACKGROUND_COLOUR | USINT  | USINT#1       | 1: AID_SKM_BACKGROUND_COLOUR – Background colour index.       |

### **Adapters**

None.

## Functionality

AID_SKM encapsulates constant definitions used for encoding and decoding soft key mask object attributes. The sole constant `BACKGROUND_COLOUR` represents the attribute identifier for the background colour index of the soft key mask, aligning with the ISOBUS standard.

## Technical Features

- **Global Scope:** The constant is declared as `VAR_GLOBAL CONSTANT`, making it available in all POUs and configurations within the project.
- **Standard Compliance:** The attribute identifier follows the ISOBUS 11783‑14 numbering scheme, facilitating interoperability with other virtual terminal implementations.
- **Type Safety:** The constant is explicitly typed as `USINT` (unsigned short integer), preventing inadvertent type mismatches.

## State Overview

As a global constants block, AID_SKM is stateless. Its value is fixed at initialization and remains unchanged during runtime.

## Application Scenarios

- **VT Screen Development:** Use `AID_SKM.BACKGROUND_COLOUR` when constructing or modifying soft key masks in order to set the background colour attribute.
- **Protocol Encoding:** When packing or unpacking ISOBUS commands that involve soft key mask attributes, this constant provides the correct attribute ID for the background colour.
- **Configuration Consistency:** Replacing hard‑coded numeric values with named constants enhances code readability and reduces errors during maintenance.

## Comparison with Similar Blocks

Unlike function blocks that process events or data, AID_SKM is a global constant container. Similar global constant blocks may exist for other object types (e.g., `AID_KI` for key masks), but they share the same principle: defining immutable attribute identifiers in a centralized location. Compared to using literals directly, a dedicated constant block improves traceability and simplifies future updates.

## Conclusion

AID_SKM serves as a well‑defined source for the soft key mask background colour attribute identifier. By using this global constant, developers ensure compliance with the ISOBUS standard while maintaining clean and maintainable code within 4diac‑IDE environments.
