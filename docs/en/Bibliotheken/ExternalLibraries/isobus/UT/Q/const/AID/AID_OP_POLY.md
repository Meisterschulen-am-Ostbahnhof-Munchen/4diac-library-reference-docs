# AID_OP_POLY

![AID_OP_POLY](./AID_OP_POLY.svg)

* * * * * * * * * *
## Introduction
The `AID_OP_POLY` global constants set defines the attribute identifiers (IDs) for an **Output Polygon** object in the ISO 11783 (ISOBUS) protocol. These constants are used in the object pool definition to reference the geometric and rendering properties of a polygon, enabling standardized communication between tractor-implements and the virtual terminal. The set provides symbolic names for the numeric attribute IDs, improving code readability and maintainability.

## Interface Structure
`AID_OP_POLY` is a **global constant set** within the 4diac‑IDE environment. It does not expose any operational interface, such as event inputs/outputs, data inputs/outputs, or adapters. Instead, it provides compile‑time constant values that can be imported and used anywhere in the automation project.

### **Event Inputs**
None – the constant set is passive and does not react to events.

### **Event Outputs**
None – no events are generated.

### **Data Inputs**
None – values are not dynamically changeable.

### **Data Outputs**
None – constants are only read at build time.

### **Adapters**
None – no adapters are required or provided.

## Functionality
The constants in `AID_OP_POLY` represent the attribute IDs defined by the ISOBUS standard for an **Output Polygon** object. Each ID is a fixed numeric value (USINT) that corresponds to a specific attribute in the object’s definition. By using these symbolic names, developers can avoid hard‑coding magic numbers and ensure compliance with the standard.
The set includes attributes for:
- **Width** of the polygon outline (in pixels)
- **Height** of the polygon (in pixels)
- **Line Attributes** – reference to a Line Attributes object (ID)
- **Fill Attributes** – reference to a Fill Attributes object (ID)
- **Polygon Type** – convex, non‑convex, complex, or open

These constants are typically used in conjunction with the `AID_OP_*` series to build complete ISOBUS object pools.

## Technical Features
- **DataType**: All constants are of type `USINT` (unsigned short integer, 8‑bit) with initial values from 1 to 5.
- **Namespace**: The constants reside in the package `isobus::UT::Q::const::AID`.
- **Scope**: Declared as `GLOBALCONSTANT` entities, they are globally accessible within the 4diac project after the defining library is imported.
- **Compatibility**: Compliant with IEC 61499‑1 and the ISOBUS object pool specification.

| Constant      | Value | Description                                                                 |
|---------------|-------|-----------------------------------------------------------------------------|
| `WIDTH`       | `USINT#1` | Attribute ID for polygon width in pixels.                                  |
| `HEIGHT`      | `USINT#2` | Attribute ID for polygon height in pixels.                                 |
| `LINE_ATT`    | `USINT#3` | Object ID of a Line Attributes object for the polygon's outline.           |
| `FILL_ATT`    | `USINT#4` | Object ID of a Fill Attributes object for the polygon's interior.          |
| `POLYGON_TYPE`| `USINT#5` | Polygon type: 0=Convex, 1=Non‑Convex, 2=Complex, 3=Open.                   |

## State Overview
The `AID_OP_POLY` constant set is **stateless**. It contains only constant values that are evaluated at compile time; there is no runtime state machine or dynamic behavior. The constants are immutable and do not change during execution.

## Application Scenarios
- **ISOBUS Object Pool Definition**: Used when defining an Output Polygon within a Virtual Terminal object pool, to set attributes like outline width, fill style, and polygon type.
- **Custom Implementations**: Developers implementing ISOBUS‑compliant control systems can include these constants to avoid errors in attribute ID assignments.
- **Tooling and Simulation**: May be used by simulation and test tools to validate object pool structures.

## Comparison with Similar Blocks
In the 4diac‑IDE environment, several global constant sets exist for different object types (e.g., `AID_OP_LINE`, `AID_OP_RECT`). Like `AID_OP_POLY`, they provide symbolic names for attribute IDs of their respective geometric objects. The structure is analogous, but the actual attribute IDs and meanings differ. For example, `AID_OP_RECT` includes attributes for corner radius and line style, while `AID_OP_POLY` focuses on polygon‑specific properties like polygon type and width/height.

## Conclusion
The `AID_OP_POLY` global constant set is a convenient and standard‑compliant way to reference the attribute IDs of an ISOBUS Output Polygon object. By using the symbolic names instead of raw numbers, IEC 61499 applications become more readable, less error‑prone, and easier to maintain. The constant set is stateless, integrates seamlessly with 4diac‑IDE, and directly supports the creation of robust ISOBUS object pools.