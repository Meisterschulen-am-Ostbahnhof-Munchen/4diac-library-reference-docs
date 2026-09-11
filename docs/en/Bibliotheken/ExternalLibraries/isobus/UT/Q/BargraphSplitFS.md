# BargraphSplitFS

![BargraphSplitFS](./BargraphSplitFS.svg)

* * * * * * * * * *

## Introduction

The **BargraphSplitFS** function block visualizes a signed REAL value on a pair of adjacent Linear Bar Graphs that meet at a shared zero point. The left bar graph renders the magnitude of negative values, while the right bar graph renders the magnitude of positive values. This arrangement provides an intuitive "split" display where the user sees a single continuous bar that extends from the center to either side, depending on the sign of the input value.

The block wraps two `Q_NumericValue_PHYS` instances (ISO 11783-6, clause F.22) that command the two bar graph objects defined in Annex B.11.3. The object configuration is provided once at initialization and remains fixed for the lifetime of the block instance.

## Interface Structure

### **Event Inputs**

| Event     | Description                                                | With               |
|-----------|------------------------------------------------------------|--------------------|
| `INIT`    | Service initialization. Snapshots the `stObj` configuration.| `stObj`            |
| `REQ`     | Displays a new signed value on the split bar graph.        | `rValue`           |

### **Event Outputs**

| Event   | Description                                              | With                                                              |
|---------|----------------------------------------------------------|-------------------------------------------------------------------|
| `INITO` | Initialization confirm. Emitted after both internal bar graph services are initialized. | – |
| `CNF`   | Confirmation of the requested display service. Emitted after both sides have processed the value. | `STATUSRight`, `s16resultRight`, `xOverRight`, `STATUSLeft`, `s16resultLeft`, `xOverLeft` |

### **Data Inputs**

| Data     | Type                                          | Description                                                                                             |
|----------|-----------------------------------------------|---------------------------------------------------------------------------------------------------------|
| `stObj`  | `isobus::utils::bargraph::BargraphSplit_S`    | Split-bargraph object pool properties (left/right bar graph references, shared magnitude bounds). Snapshotted once at `INIT`. |
| `rValue` | `REAL`                                        | Signed physical value to display. May originate from any source, not limited to a `NumericValue_PHYS`.    |

### **Data Outputs**

| Data            | Type     | Description                                                                                              |
|-----------------|----------|----------------------------------------------------------------------------------------------------------|
| `STATUSRight`   | `STRING` | Service status – passthrough from the internal right-side `Q_NumericValue_PHYS`.                          |
| `s16resultRight`| `INT`    | Return value – passthrough from the internal right-side `Q_NumericValue_PHYS`.                            |
| `xOverRight`    | `BOOL`   | `TRUE` when the positive input value exceeded `stObj.r32MaxMagnitude` and the right side was clamped.     |
| `STATUSLeft`    | `STRING` | Service status – passthrough from the internal left-side `Q_NumericValue_PHYS`.                           |
| `s16resultLeft` | `INT`    | Return value – passthrough from the internal left-side `Q_NumericValue_PHYS`.                             |
| `xOverLeft`     | `BOOL`   | `TRUE` when the negated negative input value exceeded `stObj.r32MaxMagnitude` and the left side was clamped.|

### **Adapters**

None.

## Functionality

The block operates in two phases: initialization and value display.

**Initialization (INIT):**

1. The `stObj` structure is snapshotted into an internal copy using an `F_MOVE` block (`Snap`).
2. From the snapshot, the right-side configuration (`stRight`) is forwarded to the internal right `Q_NumericValue_PHYS`, and the left-side configuration (`stLeft`) is forwarded to the internal left `Q_NumericValue_PHYS`.
3. Both internal instances are initialized sequentially. The `INITO` event confirms completion.

**Value display (REQ):**

The signed `rValue` is processed along two parallel paths:

* **Right side (positive branch):** `rValue` is fed directly into a clamping stage (`ClampRight`) that bounds the value to `[stObj.r32MinMagnitude, stObj.r32MaxMagnitude]`. The clamped magnitude is then written to the right bar graph via `Q_NumericValue_PHYS`. If `rValue > r32MaxMagnitude`, `xOverRight` is set to `TRUE`.
* **Left side (negative branch):** `rValue` is first negated by an `F_MUL` with `-1.0`, producing the magnitude of the negative input. This value is clamped with the same bounds (`ClampLeft`) and written to the left bar graph. If `-rValue > r32MaxMagnitude`, `xOverLeft` is set to `TRUE`.

Both branches are processed using a strict sequential event chain: `REQ → ClampRight → NumRight → NegateValue → ClampLeft → NumLeft → CNF`. This guarantees deterministic ordering and ensures both bar graphs are updated before the confirmation event is emitted.

The clamping stage maps `r32MinMagnitude` (typically 0) to the empty state, while `r32MaxMagnitude` corresponds to the full-scale fill of the bar graph. Values below `r32MinMagnitude` are clamped upward to the minimum, and values above `r32MaxMagnitude` are clamped to full scale.

There are deliberately no `xUnder` outputs. A magnitude falling below `r32MinMagnitude` on the currently inactive side is the normal resting state during ordinary operation and does not constitute a diagnostic condition.

## Technical Features

* **Object snapshotting:** The `BargraphSplit_S` structure is captured once at `INIT` via `F_MOVE`, making the block immune to later changes of the source object references.
* **Reusable internal services:** The block leverages standard `Q_NumericValue_PHYS` (ISO 11783-6 F.22) and `F_ClampReal` building blocks, ensuring consistent behavior with other parts of the ISOBUS object pool.
* **Shared bounds:** Both bar graph sides use the same `r32MinMagnitude` and `r32MaxMagnitude` from the snapshot, guaranteeing a symmetric, coherent presentation around the zero point.
* **Deterministic event sequencing:** The internal event chain guarantees that both sides are fully processed in a fixed order before `CNF` is emitted.
* **Over-range indication:** Dedicated boolean outputs report magnitude overruns independently for each side, allowing the application to react (e.g., log a diagnostic or highlight the value).
* **No under-range diagnostic:** By design, the block omits under-range flags to avoid spurious diagnostics during normal zero-crossing operation.

## State Overview

The block is stateless from the application perspective. Its internal lifecycle is:

| State/Phase | Trigger          | Actions                                                                 |
|-------------|------------------|-------------------------------------------------------------------------|
| Idle        | –                | Waiting for `INIT` or `REQ`.                                             |
| Initializing| `INIT`           | Snapshot `stObj`, initialize right instance, then left instance, emit `INITO`. |
| Processing  | `REQ`            | Clamp right branch, update right bar graph, negate/clamp left branch, update left bar graph, emit `CNF`. |
| Ready       | `INITO` / `CNF`  | Return to idle, awaiting next service request.                           |

## Application Scenarios

* **Tank level / fill indication with direction:** Displaying a differential value (e.g., pressure difference, flow balance) where positive values fill the right bar and negative values fill the left bar.
* **Dual-directional machine parameter display:** Showing joystick deflection, steering angle, or actuator position in both directions from a neutral center point.
* **Operator diagnostics:** In ISOBUS-based agricultural machinery, presenting signed physical quantities (speed deviation, draft force imbalance) in a compact split-bar format.
* **Generic human-machine interface:** Any HMI requiring a signed value to be rendered as two adjacent magnitude bars with a common zero, using standard ISO 11783-6 bar graph objects.

## Comparison with Similar Blocks

| Feature                      | BargraphSplitFS                          | Simple single bar graph FB                         |
|------------------------------|------------------------------------------|----------------------------------------------------|
| Display of signed values     | Yes – split into two directional bars    | Usually requires absolute value pre-processing     |
| Zero point                   | Shared central zero between two bars     | Typically at one end of the bar                    |
| Over-range reporting         | Per-side via `xOverRight` / `xOverLeft`  | Often single over-range flag                      |
| Under-range reporting        | None (intentional)                       | Often present, may cause false alarms on inactive side |
| Configuration snapshotting   | Yes, at `INIT`                           | Varies                                            |

Unlike a single `Q_NumericValue_PHYS` block, which can only display an unsigned magnitude, BargraphSplitFS preserves the sign information natively by assigning the value to one of two bar graph objects. This removes the need for external sign-detection and absolute-value logic in the application.

## Conclusion

The BargraphSplitFS function block provides a robust and reusable solution for displaying signed REAL values on a pair of adjacent linear bar graphs. By combining ISO 11783-6 compliant `Q_NumericValue_PHYS` services with systematic clamping and deterministic event sequencing, it delivers a clean, symmetric visual representation where the zero point is always shared between the two bars. Its per-side over-range flags, configuration snapshotting, and deliberate omission of under-range diagnostics make it well suited for agricultural and industrial HMIs that require clear directional magnitude indication.
