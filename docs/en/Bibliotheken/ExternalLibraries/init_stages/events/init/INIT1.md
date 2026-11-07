# INIT1

![INIT1](./INIT1.svg)

![INIT1_network](./INIT1_network.svg)

* * * * * * * * * *

## Introduction

`INIT1` is stage 1 of the staged initialization (library `init_stages`, package `events::init`). The function block is the counterpart to an [`E_TRIG`](../../../../StandardLibraries/events/E_TRIG.md) with the event type `E1Init`: the `E_TRIG` triggers the unconnected input `INIT1`, and `INIT1O` is connected to the `INIT` of the target. Its position in the chain of `E_TRIG` blocks is set by the wiring in `EMB_RES`; it is not yet wired in the example chain. The block contains no logic.

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
| ---- | ---- | ------- |
| INIT1 | E1Init | Initialization Request |
| REQ | Event | Service Request |

### **Event Outputs**

| Name | Type | Comment |
| ---- | ---- | ------- |
| INIT1O | E1Init | Initialization Confirm |
| CNF | Event | Confirmation of Requested Service |

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

Pure event connections in the inner FB network:

- `INIT1` → `INIT1O`
- `REQ` → `CNF`

## Usage

1. Place `INIT1` directly next to the target that is to be initialized in this stage.
2. Connect `INIT1O` to the `INIT` of the target. Nothing has to be wired through the hierarchy.
3. In `EMB_RES`, add an `E_TRIG` with `EVENTTYPE` `'E1Init'` to the chain of stages (see [`init_stages`](../../index.md)).

## Technical Details

- Composite function block without data and without internal state.
- The event type `E1Init` is independent; only an `E_TRIG` with this `EVENTTYPE` triggers the input.
- A stage counts as done when all its inputs have been triggered, not when the targets have finished initializing.
- See also [`INIT`](INIT.md) (standard stage, `EInit`).

## Conclusion

`INIT1` connects initialization stage 1 to its target without wiring through the hierarchy.
