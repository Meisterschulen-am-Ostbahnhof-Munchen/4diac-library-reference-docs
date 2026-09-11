# AID_TYPE

![AID_TYPE](./AID_TYPE.svg)

* * * * * * * * * *

## Introduction

`AID_TYPE` is a global constant definition used in isobus-based applications. It defines a single constant named `OBJ_TYPE` of type `USINT` with an initial value of `0`. This constant represents the Object Type attribute identifier for an AID (Application Identification) message, as specified in the ISO 11783 (ISOBUS) standard.

## Interface Structure

Since `AID_TYPE` is a GlobalConstants block, it does not contain typical FB interfaces such as event inputs, data inputs, or adapters. Instead, it exposes a single global constant:

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

None.

However, the constant `OBJ_TYPE` is globally accessible to any FB or application that needs to reference the Object Type identifier.

## Functionality

`AID_TYPE` provides a standardized, constant value for the Object Type attribute ID in ISOBUS communication. The constant `OBJ_TYPE` is assigned the value `0`, which corresponds to the `AID_TYPE_OBJ_TYPE` attribute. This constant is used to identify the type of object in AID messages, helping to ensure consistency across different implementations.

## Technical Features

- **Type:** `USINT` (Unsigned Short Integer, 8-bit)
- **Initial Value:** `0` (using IEC 61131-3 notation `USINT#0`)
- **Scope:** Global constant, accessible throughout the entire 4diac application.
- **Package Context:** Defined within the package `isobus::UT::Q::const::AID`.

## State Overview

Global constants have no runtime state. The value of `OBJ_TYPE` is fixed at `0` and cannot be modified during execution.

## Application Scenarios

- **ISOBUS Message Handling:** When constructing or parsing AID messages, the constant can be used to set or compare the Object Type field.
- **System Consistency:** Provides a single source of truth for the Object Type identifier, reducing the risk of hardcoding errors across multiple FBs or modules.

## Comparison with Similar Blocks

Unlike function blocks or adapters, GlobalConstants blocks do not process events or data. Their purpose is to define reusable constants. Similar global constant blocks might define other attribute IDs (e.g., `AID_ATTRIBUTE`, `AID_VALUE`) but `AID_TYPE` specifically focuses on the Object Type attribute.

## Conclusion

`AID_TYPE` is a simple, yet essential global constant definition that standardizes the Object Type attribute ID for ISOBUS AID messages. By using this constant, developers can avoid magic numbers and improve maintainability of their 4diac applications.
