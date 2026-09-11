# Industry_Groups

![Industry_Groups](./Industry_Groups.svg)

* * * * * * * * * *
## Introduction
This is a global constants group in the 4diac IDE, part of the ISOBUS library. It defines 8 industry group identifiers used in ISOBUS parameter group numbers (PGNs). These constants are of type BYTE and range from 0 to 7, representing different industrial sectors such as agricultural, construction, marine, etc.

## Interface Structure
As a GlobalConstants type, it has no event inputs, event outputs, data inputs, data outputs, or adapters. Instead, it provides a set of global constants that can be referenced by other function blocks. The defined constants are listed below:

| Constant Name | Type | Initial Value | Comment |
|---------------|------|---------------|---------|
| IG_GLOBAL | BYTE | 0 | Industry Group: Global, applies to all |
| IG_ON_HIGHWAY_EQUIPMENT | BYTE | 1 | Industry Group: On-Highway Equipment |
| IG_AGRICULTURAL_AND_FORESTRY_EQUIPMENT | BYTE | 2 | Industry Group: Agricultural and Forestry Equipment |
| IG_CONSTRUCTION_EQUIPMENT | BYTE | 3 | Industry Group: Construction Equipment |
| IG_MARINE | BYTE | 4 | Industry Group: Marine |
| IG_INDUSTRIAL_PROCESS_CONTROL_STATIONARY | BYTE | 5 | Industry Group: Industrial-Process Control-Stationary (Gen-Sets) |
| IG_RESERVED_FOR_FUTURE_ASSIGNMENT_BY_SAE | BYTE | 6 | Industry Group: Reserved for future assignment by SAE |
| IG_RESERVED_FOR_FUTURE_ASSIGNMENT_BY_SAE_7 | BYTE | 7 | Industry Group: Reserved for future assignment by SAE |

### **Event Inputs**
No event inputs are defined.

### **Event Outputs**
No event outputs are defined.

### **Data Inputs**
No data inputs are defined.

### **Data Outputs**
No data outputs are defined.

### **Adapters**
No adapters are defined.

## Functionality
The constants defined in this group are used to identify the industry group in ISOBUS PGNs. Each PGN includes a 3-bit industry group field that determines the applicable scope for the message. By using these named constants, application developers can avoid magic numbers and improve code readability and maintainability.

## Technical Features
- All constants are of type BYTE and are declared as global constants.
- Values range from 0 to 7, with 6 and 7 reserved for future assignment by SAE.
- The constants are compiled into the package `isobus::pgn::const`, making them accessible across the entire ISOBUS library.

## State Overview
Not applicable; this is a constant definition type and does not contain state machines or behavior.

## Application Scenarios
These constants are primarily used in the development of ISOBUS-compliant agricultural, construction, and forestry machinery control systems. They are typically referenced when constructing or parsing PGNs to set or interpret the industry group field. For example, an ECU in an agricultural tractor would use `IG_AGRICULTURAL_AND_FORESTRY_EQUIPMENT` (value 2) when sending PGNs.

## Comparison with Similar Blocks
Similar global constants groups exist for other ISOBUS enumerations, such as protocol versions, function codes, or address claims. This particular group focuses specifically on industry group identification, making it distinct from other constant sets. Unlike function blocks, it does not provide any processing logic but merely a repository of standard constants.

## Conclusion
The `Industry_Groups` global constants group is an essential building block for ISOBUS application development, providing well-defined and standardized identifiers for industry sectors. Its use enhances code clarity and ensures compliance with the ISOBUS standard.