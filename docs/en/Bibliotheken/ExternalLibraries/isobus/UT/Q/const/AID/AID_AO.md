# AID_AO

![AID_AO](./AID_AO.svg)

* * * * * * * * * *
## Introduction

AID_AO is a global constant definition block used in the 4diac IDE for the ISO 11783 (ISOBUS) Virtual Terminal protocol. It defines the attribute identifiers (AID) for animation objects (AO) within an ISOBUS Universal Terminal (UT) object pool. The block provides symbolic constant names that map to numerical attribute IDs, making configuration code more readable and maintainable when working with animation objects.

## Interface Structure

AID_AO is a GlobalConstants type, not a function block or adapter. It contains no event or data interfaces. Instead, it exposes a set of global constant values of type USINT, available throughout the package `isobus::UT::Q::const::AID`.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

The following constants are defined as global, read-only values:

| Name | Type | Value | Description |
|------|------|-------|-------------|
| WIDTH | USINT | 1 | AID_AO_WIDTH – Width in pixels. |
| HEIGHT | USINT | 2 | AID_AO_HEIGHT – Height in pixels. |
| REFRESHINT | USINT | 3 | AID_AO_REFRESHINT – Refresh interval. |
| VALUE | USINT | 4 | AID_AO_VALUE – Current value. |
| ENABLED | USINT | 5 | AID_AO_ENABLED – 0 = Stopped, 1 = Animating. |
| FICHILDINDEX | USINT | 6 | AID_AO_FICHILDINDEX – First child index. |
| LACHILDINDEX | USINT | 7 | AID_AO_LACHILDINDEX – Last child index. |
| DEFCHILDINDEX | USINT | 8 | AID_AO_DEFCHILDINDEX – Default child index. |
| OBJECTS | USINT | 9 | AID_AO_OBJECTS – Options: Bit 0 = Animation Sequence (0 = Single Shot, 1 = Loop); Bits 1–2 = Disabled Behaviour (0 = Pause, 1 = Reset to First, 2 = Default Object, 3 = Blank). |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The AID_AO global constants serve as named references for the attribute IDs that configure and control ISOBUS animation objects. Each constant corresponds to a specific attribute used in the UT object pool:

- **WIDTH** and **HEIGHT** define the pixel dimensions of the animation object.
- **REFRESHINT** specifies the refresh interval for animation updates.
- **VALUE** holds the current animation value.
- **ENABLED** controls whether the animation is stopped or running.
- **FICHILDINDEX**, **LACHILDINDEX**, and **DEFCHILDINDEX** identify the first, last, and default child object indices respectively.
- **OBJECTS** packs two configuration options into one byte: the animation sequence mode (single shot or loop) and the behaviour when the animation is disabled (pause, reset to first, show default object, or blank).

These constants improve code readability and reduce the risk of errors that would occur if raw numeric IDs were used directly.

## Technical Features

- Compliant with IEC 61499-1 standard for GlobalConstants.
- All constants are of type USINT (8-bit unsigned integer).
- Values are fixed at compile time and cannot be modified during runtime.
- The block is defined within the package namespace `isobus::UT::Q::const::AID`.
- The OBJECTS constant demonstrates bitwise encoding of multiple configuration flags into a single byte.

## State Overview

Not applicable. AID_AO is a passive constant definition and does not implement stateful or sequential behaviour.

## Application Scenarios

AID_AO is applied in ISOBUS Virtual Terminal applications where animation objects are configured and dynamically controlled. Typical use cases include:

- Setting the dimensions of an animation object via WIDTH and HEIGHT.
- Configuring the refresh rate with REFRESHINT.
- Starting or stopping animations using ENABLED.
- Reading or setting the current animation value via VALUE.
- Managing child object ordering with FICHILDINDEX, LACHILDINDEX, and DEFCHILDINDEX.
- Selecting animation mode (loop vs. single shot) and disabled behaviour through OBJECTS.

## Comparison with Similar Blocks

AID_AO belongs to a family of attribute ID constant definitions used in ISOBUS VT contexts. Unlike function blocks, which encapsulate behaviour and state transitions, AID_AO provides only static constant values. It is comparable to other AID constant groups (e.g., for object types or control functions) but focuses specifically on animation objects. Its advantage lies in providing a single, consistent source of truth for animation attribute identifiers, eliminating magic numbers in application code.

## Conclusion

AID_AO is a compact and well-structured global constant set that centralises all animation object attribute identifiers in an ISOBUS environment. By offering symbolic names for numeric IDs and encoding complex options into a single byte, it enhances code clarity, maintainability, and correctness. It is a fundamental building block for developers implementing ISOBUS Virtual Terminal animations within the 4diac IDE.