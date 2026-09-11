# AID_GD

![AID_GD](./AID_GD.svg)

* * * * * * * * * *

## Introduction

The `AID_GD` global constants library defines attribute identifiers for graphic data (PNG) objects. It is used within the ISO‑bus (ISOBUS) ecosystem to identify and manage graphic data attributes, specifically for PNG image encoding. The constants are defined as global constant variables, providing a single, standardized reference for graphic data attributes in accordance with ISO 11783‑14 (ISOBUS) conventions.

This component is part of the `isobus::UT::Q::const::AID` package and is intended to be used by function blocks and other modules that need to reference graphic data attribute identifiers. It contains a single constant, `FORMAT`, which specifies the graphic type. The initial value `USINT#1` indicates the PNG format, restricted to 32‑bit RGBA maximum.

## Interface Structure

Since `AID_GD` is a global constants library, it does not provide event, data, or adapter interfaces. It defines only constant values accessible globally.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None. The constant value `FORMAT` is a global constant, not a data input.

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The primary purpose of `AID_GD` is to define a global constant, `FORMAT`, that stores the graphic data format identifier. This constant is used to indicate the encoding type of graphic data (e.g., PNG) within ISOBUS applications. By centralizing this value, consistency and maintainability are ensured across all modules that reference graphic data attributes.

The constant is defined as:

- **`FORMAT`** (`USINT`, initial value `USINT#1`) – Specifies the graphic type. The value `1` corresponds to the PNG format, as per the comment: *“1: AID_GD_FORMAT - Graphic type: 0 = PNG, restricted to 32bit RGBA maximum.”* Note: The comment appears to have a typo (0 vs 1), but the actual initial value is `1`, and the intended meaning is that PNG is the graphic type.

## Technical Features

- **Global Constant** – The constant is declared in a global constants block, making it accessible throughout the entire application without requiring instantiation.
- **Data Type** – `USINT` (unsigned short integer, 8‑bit) ensures a small and efficient representation.
- **Standard Compliance** – The package name `isobus::UT::Q::const::AID` indicates alignment with ISOBUS standardized attribute IDs.
- **Initialization** – The constant is initialized to `USINT#1`, which is the predefined value for the PNG format.

## State Overview

As a constants library, `AID_GD` does not maintain any internal state. It consists solely of compile‑time constants that are immutable during runtime. No state transitions or lifecycle management are involved.

## Application Scenarios

Typical use cases for `AID_GD` include:

- **ISOBUS Object Pool Generation** – When constructing object pools for virtual terminals, graphic data (e.g., icons or images) must be associated with format identifiers. `AID_GD` provides the correct attribute ID to be referenced.
- **Graphic Data Handling** – Function blocks responsible for processing or displaying graphic data can use `FORMAT` to determine whether the incoming data is PNG or another supported format.
- **Inter‑Module Coordination** – Since the constant is global, multiple blocks can access the same format definition without risk of inconsistency.

## Comparison with Similar Blocks

Compared to other global constant libraries in the ISOBUS environment (e.g., attribute IDs for object types, color definitions), `AID_GD` is specifically focused on graphic data format attributes. While similar libraries might contain multiple constants for various attribute IDs, `AID_GD` currently exposes only one constant, `FORMAT`. This makes it lightweight and specialized. It does not compete with function blocks that perform processing logic; rather, it supplies a static configuration value for those blocks.

## Conclusion

`AID_GD` is a minimalistic but essential global constants definition that standardizes the graphic data format attribute identifier for PNG objects in ISOBUS applications. By using this constant, developers ensure that all references to graphic data formats remain consistent and easily maintainable. Its simple design—a single immutable value—makes it a reliable and efficient building block for larger ISOBUS‑based systems.
