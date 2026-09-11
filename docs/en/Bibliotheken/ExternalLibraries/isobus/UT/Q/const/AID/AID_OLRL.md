# AID_OLRL

![AID_OLRL](./AID_OLRL.svg)

* * * * * * * * * *
## Introduction
The `AID_OLRL` global constant set defines a collection of attribute identifiers used in the context of object label reference lists within the ISOBUS UT (Universal Terminal) protocol. These constants serve as predefined numeric values that facilitate the identification and handling of labelled objects in agricultural machinery communication. This constant set is part of the `isobus::UT::Q::const::AID` namespace and provides a single, clearly named constant for use in application logic.

## Interface Structure
As a global constant set, `AID_OLRL` does not expose events, data inputs/outputs, or adapters. Instead, it provides a static value that can be referenced by other function blocks or applications.

### **Event Inputs**
None.

### **Event Outputs**
None.

### **Data Inputs**
None.

### **Data Outputs**
None.

### **Adapters**
None.

## Functionality
The `AID_OLRL` constant set defines a single constant:

| Constant Name | Data Type | Initial Value | Description |
|---------------|-----------|---------------|-------------|
| `NUMB_LABELLED_OBJ` | `USINT` | `USINT#1` | Attribute ID for the number of labelled objects that follow in an object label reference list. |

This constant is intended to be used when constructing or parsing object label reference lists, ensuring consistent identification of the "number of labelled objects" field across different implementations.

## Technical Features
- **Data Type**: `USINT` (unsigned short integer), occupying 1 byte.
- **Initial Value**: `1`, representing the standard attribute ID for the number of labelled objects.
- **Namespace**: `isobus::UT::Q::const::AID` (accessed as `AID_OLRL.NUMB_LABELLED_OBJ`).
- **Compliance**: Aligns with ISO 11783 (ISOBUS) standards for UT functional objects.

## State Overview
Not applicable – this is a constant set and does not maintain any runtime state.

## Application Scenarios
- Used when decoding or encoding object pool transfer messages that include object label reference lists.
- Helps to write robust code that avoids hard-coded attribute IDs.
- Serves as a reference for developers implementing ISOBUS UT clients or servers.

## Comparison with Similar Blocks
Similar global constant sets exist for other attribute groups (e.g., `AID_OL` for object labels). Unlike function blocks, this constant set provides no processing capability; it simply offers named numeric constants. When compared to direct literals, using the constant improves code readability and maintainability.

## Conclusion
The `AID_OLRL` global constant set is a simple, yet essential, component for applications dealing with ISOBUS object label reference lists. By defining the `NUMB_LABELLED_OBJ` constant, it standardizes the representation of this attribute ID, reducing the risk of errors and enhancing code clarity.