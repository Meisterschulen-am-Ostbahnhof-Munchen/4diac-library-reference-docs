# AID_CP

![AID_CP](./AID_CP.svg)

* * * * * * * * * *
## Introduction
The **AID_CP** global constants definition provides a set of named constant values used as attribute identifiers for colour palette objects in the ISOBUS (ISO 11783) protocol context. It is part of the `isobus::UT::Q::const::AID` package and contains a single constant, `OPTIONS`, which is used to reference the options attribute of a colour palette object.

## Interface Structure
Since `AID_CP` is a global constants declaration (not a function block, adapter, or subapplication), it does not define event or data interfaces in the traditional sense. However, for consistency with the documentation template, the following sections explicitly state that no I/O elements are present.

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

The sole constant defined within `AID_CP` is:
- **OPTIONS** (USINT) – Value `USINT#1`. This constant represents the attribute ID for the “Options” field of a colour palette object. According to the ISOBUS specification, the options attribute is always set to 0, but the constant is defined to standardise access to it in code.

## Functionality
The purpose of `AID_CP` is to provide a central, reusable definition for the colour palette object attribute identifier. By using this constant, developers can avoid hard‑coding numeric values in their logic and can ensure consistency across the system. The constant `OPTIONS` is intended to be used wherever the options attribute of a colour palette object needs to be referenced, e.g., in get/set operations or in communication frames.

## Technical Features
- **Constant Data Type:** `USINT` (unsigned short integer, 8–bit).
- **Initial Value:** `1` (although the comment notes that the actual option value is always 0; the constant serves as an attribute ID, not as a value).
- **Namespace:** The constant is defined within the `isobus::UT::Q::const::AID` package, aligning with ISOBUS utility layer conventions.
- **Packaging:** The XML declaration includes a standard identification and version information, making it suitable for exchange in 4diac‑ide projects.

## State Overview
As a global constants object, `AID_CP` does not have internal states or transitions. It is a compile‑time constant set; its value is fixed at design time and does not change during runtime.

## Application Scenarios
Typical use cases include:
- Implementing ISOBUS colour palette management in agricultural machinery control systems.
- Providing a shared constant for attribute identification across multiple function blocks or applications that handle colour palette objects.
- Serving as a reference in test and simulation environments where colour palette attributes are accessed.

## Comparison with Similar Blocks
Unlike function blocks (which encapsulate behaviour and have event/data interfaces) or adapters (which define communication patterns), `AID_CP` is a passive data container. It cannot be instantiated or executed; it simply provides named constants. Similar global constants objects in the ISOBUS domain exist for other object types (e.g., AID_GA for geometric attributes), each following the same pattern of defining attribute IDs as named constants.

## Conclusion
The `AID_CP` global constants object defines a single, well‑documented constant (`OPTIONS`) for colour palette object attribute identification. It contributes to code clarity and maintainability by centralising an otherwise numeric value in a descriptive, reusable form. This approach aligns with best practices for ISOBUS development in the 4diac environment.