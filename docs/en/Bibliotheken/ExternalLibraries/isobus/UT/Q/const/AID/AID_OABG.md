# AID_OABG

![AID_OABG](./AID_OABG.svg)

* * * * * * * * * *
## Introduction

The `AID_OABG` global constant set provides symbolic names for the attribute identifiers (AIDs) of an **Output Arched Bar Graph (OABG)** object as specified by the ISOBUS Virtual Terminal (VT) standard. Instead of using magic numbers, applications can reference these constants to set or modify OABG attributes, improving code readability and maintainability. This constant set is defined in the package `isobus::UT::Q::const::AID` and is intended for use in IEC 61499 applications running on 4diac-ide.

## Interface Structure

This global constant set does not define a standard function block interface with event or data inputs/outputs. It is a collection of compile-time constants that can be used anywhere a USINT attribute ID is required. The following sections are included for completeness, but they are not applicable to a constant set.

### **Event Inputs**

None – no event inputs are defined.

### **Event Outputs**

None – no event outputs are defined.

### **Data Inputs**

None – no data inputs are defined.

### **Data Outputs**

None – no data outputs are defined.

### **Adapters**

None – no adapters are defined.

## Functionality

Each constant in `AID_OABG` corresponds to a specific attribute of an OABG object, as defined by the ISOBUS standard. The following table lists all constants, their attribute identifiers, and their meaning:

| Constant Name           | Value | Attribute Description |
|-------------------------|-------|-----------------------|
| `WIDTH`                 | 1     | Width in pixels       |
| `HEIGHT`                | 2     | Height in pixels      |
| `COLOUR`                | 3     | Colour index          |
| `TARGET_LINE_COLOUR`    | 4     | Target line colour    |
| `OPTIONS`               | 5     | Bitmask: Bit 0=Draw border, Bit 1=Draw target line, Bit 2=Undefined (set to 0), Bit 3=Bar graph type (0=filled, 1=single line), Bit 4=Deflection (0=anticlockwise, 1=clockwise) |
| `START_ANGLE`           | 6     | Start angle in degrees |
| `END_ANGLE`             | 7     | End angle in degrees   |
| `BAR_GRAPH_WIDTH`       | 8     | Bar graph width       |
| `MIN_VALUE`             | 9     | Minimum scaled value  |
| `MAX_VALUE`             | 10    | Maximum scaled value  |
| `VARIABLE_REF`          | 11    | Object ID of a Variable object |
| `TARGET_VAL_VAR_REF`    | 12    | Object ID of a Variable object for target value |
| `TARGET_VALUE`          | 13    | Target value          |
| `VALUE`                 | 14    | Current value         |

These constants are all of type `USINT` and are initialized with their numeric attribute ID values. They are intended to be used in conjunction with VT commands such as those that set attribute values (e.g., `SetAttribute`).

## Technical Features

- **Type:** All constants are `USINT` (unsigned 8-bit integer), matching the attribute ID type in the ISOBUS standard.
- **Package:** `isobus::UT::Q::const::AID`
- **Compliance:** The constants follow the attribute numbering defined in ISO 11783-6 for the OABG object.
- **Read‑only:** The constants are `VAR_GLOBAL CONSTANT`, meaning they cannot be modified at runtime.
- **Use in 4diac-ide:** The constants can be directly referenced in ST or FBD code, e.g., `AID_OABG.WIDTH`.

## State Overview

Not applicable. This constant set does not contain any stateful elements. It merely provides static definitions.

## Application Scenarios

- **Creating an OABG object:** When creating a new output arched bar graph on an ISOBUS VT, the application must supply attribute IDs for properties such as width, height, colours, and angles. Using `AID_OABG.*` ensures correct values.
- **Modifying OABG attributes:** When sending a `SetAttribute` command to a VT, the attribute ID parameter can be set using these constants, avoiding hard‑coded numbers.
- **Reading OABG status:** When querying the current value or target value, the attribute ID can be referenced symbolically.
- **Cross‑platform consistency:** By using a central constant set, the same attribute IDs are guaranteed across different parts of an application, reducing the risk of inconsistency.

## Comparison with Similar Blocks

Similar global constant sets exist for other VT objects, such as `AID_OARG` (Output Arched Bar Graph) or `AID_OMSG` (Output Message), each defining attribute IDs for their respective object types. Compared to using raw numeric literals, these constant sets improve code readability and maintainability. They also align with the ISOBUS standard, ensuring compatibility with various VT implementations. Unlike function blocks, they do not process data or events, but they serve as essential building blocks in the development of IEC 61499 applications for agricultural machinery.

## Conclusion

The `AID_OABG` global constant set is a simple yet valuable utility for developers working with ISOBUS Virtual Terminal objects. By providing symbolic names for all attribute IDs of an output arched bar graph, it eliminates magic numbers and promotes clear, maintainable application code. It is an integral part of the `isobus::UT::Q::const` package and should be used whenever OABG attributes are accessed in 4diac-ide projects.