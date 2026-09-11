# AID_OL

![AID_OL](./AID_OL.svg)

* * * * * * * * * *

## Introduction

`AID_OL` is a 4diac-ide global constants definition, not a function block, adapter, or subapplication. It defines a set of named `USINT` constants used as attribute IDs for ISOBUS output line objects. The constant group is part of the package `isobus::UT::Q::const::AID` and is intended to make object pool definitions more readable and maintainable.

## Interface Structure

`AID_OL` does not provide an IEC 61499 event/data interface. It is a global constant collection that can be referenced by name inside other IEC 61499 elements.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None. The constants are not runtime input signals; they are globally defined compile-time values.

### **Data Outputs**

None.

### **Adapters**

None.

## Defined Constants

| Constant | Type | Initial Value | Description |
|----------|------|---------------|-------------|
| `LINE_ATT` | `USINT` | `1` | Object ID of a Line Attributes object. |
| `WIDTH` | `USINT` | `2` | Width in pixels. |
| `HEIGHT` | `USINT` | `3` | Height in pixels. |
| `LINE_DIR` | `USINT` | `4` | Line direction: `0` = top-left to bottom-right, `1` = bottom-left to top-right. |

## Functionality

The purpose of `AID_OL` is to centralize the numeric attribute IDs for ISOBUS output line objects. Instead of using hard-coded numbers in function block logic, applications can reference these symbolic constants. This improves readability and reduces the risk of errors when multiple parts of an application need the same attribute IDs.

The constant group contains four attributes that are relevant for configuring line objects in an ISOBUS Virtual Terminal object pool:

- `LINE_ATT` identifies the line attributes object.
- `WIDTH` and `HEIGHT` describe the line dimensions in pixels.
- `LINE_DIR` defines the drawing direction of the line.

## Technical Features

- Defined as a `GlobalConstants` element in 4diac-ide.
- Uses the IEC 61499 `USINT` data type.
- Contains fixed initial values assigned at declaration.
- Packaged under `isobus::UT::Q::const::AID`.
- Can be reused across multiple FBs, subapplications, and systems.
- Provides symbolic names for numeric ISOBUS attribute IDs.

## State Overview

Not applicable. `AID_OL` is a constant definition and does not contain state machines, execution states, or dynamic runtime behavior.

## Application Scenarios

- Building ISOBUS object pools with output line objects.
- Setting line attributes such as width, height, and direction in a Virtual Terminal screen.
- Replacing magic numbers in IEC 61499 applications with meaningful symbolic constants.
- Sharing a common definition of output line attribute IDs across multiple function blocks or projects.

## Comparison with Similar Blocks

`AID_OL` can be compared with other ISOBUS attribute ID constant groups, typically named `AID_*`. While function blocks and adapters define runtime behavior, `AID_OL` only provides static constant values. Its main advantage over inline numeric literals is centralization and consistency. Similar constant groups exist for other ISOBUS object types and provide the same style of symbolic access.

## Conclusion

`AID_OL` is a compact global constants group for ISOBUS output line object attribute IDs. It defines four `USINT` constants in a named package, making them easy to reuse in 4diac-ide applications. Although it has no function block interface or runtime state, it contributes to cleaner, more maintainable ISOBUS object pool handling.
