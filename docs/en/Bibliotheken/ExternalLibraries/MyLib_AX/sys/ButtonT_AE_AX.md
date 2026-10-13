# ButtonT_AE_AX

![ButtonT_AE_AX_network](./ButtonT_AE_AX_network.svg)

* * * * * * * * * *

## Introduction

`ButtonT_AE_AX` bundles **one button (trigger)** and a `GreenWhiteBackground` behind a single bidirectional `AE_AX` plug (trigger out, state in). It is the pure HMI part without any OPC-UA reference and is reusable. The block is the button counterpart of [`SoftKeyT_AE_AX`](./SoftKeyT_AE_AX.md): `Softkey_IE` was replaced by `Button_IE`, `SK_RELEASED` by `BT_PRESSED_LATCHED`.

![ButtonT_AE_AX](ButtonT_AE_AX.svg)

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

- **u16ObjId** (`UINT`): object ID of button/background (default `ID_NULL`)

### **Data Outputs**

None.

### **Adapters**

- **OUT** (plug, `adapter::types::bidirectional::AE_AX`): trigger (out), state (in)

## Function Blocks (FBs) Used

- **`Button_UP_F1`** (`isobus::UT::io::Button::Button_IE`): parameters `QI = TRUE`, `InputEvent = BT_PRESSED_LATCHED`
- **`AE_EVENT_TO_E`** (`adapter::conversion::unidirectional::AE_EVENT_TO_E`): converts the button event into an `AE` adapter
- **`SPLIT`** (`adapter::events::bidirectional::AE_TO_AE_AX_SPLIT`): separates the bidirectional `AE_AX` plug into trigger direction and state direction
- **`GreenWhiteBackground_AX`** (SubApp `MyLib::sys::GreenWhiteBackground1_AX`): green/white background colour

## Functionality

- `Button_UP_F1.IND` → `AE_EVENT_TO_E.REQ`
- `AE_EVENT_TO_E.AE_OUT` → `SPLIT.IN`
- `SPLIT.OUT` → `OUT` (trigger to the outside, state from the outside)
- `SPLIT.AX_OUT` → `GreenWhiteBackground_AX.DI1` (the state colours the background)
- `u16ObjId` → `Button_UP_F1.u16ObjId` and `GreenWhiteBackground_AX.u16ObjId`

## Technical Details

- The same object ID serves the button and the background colour
- `BT_PRESSED_LATCHED` instead of `SK_RELEASED` of the softkey counterpart
- No OPC-UA reference, so reusable in any scenario

## Application Scenarios

- A button in the VT as a trigger whose state (e.g. from a toggle) is fed back as background colour, through a single `AE_AX` connection

## Summary

`ButtonT_AE_AX` bundles button trigger and state colour behind one `AE_AX` plug.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
