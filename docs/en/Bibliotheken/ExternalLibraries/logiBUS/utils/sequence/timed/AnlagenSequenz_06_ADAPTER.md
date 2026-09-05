# AnlagenSequenz_06_ADAPTER

![AnlagenSequenz_06_ADAPTER_network](./AnlagenSequenz_06_ADAPTER_network.svg)

* * * * * * * * * *

## Introduction

`AnlagenSequenz_06_ADAPTER` is a composite wrapper around [AnlagenSequenz_06](AnlagenSequenz_06.md): the same ring-sequencer logic for six motors, but with every data-carrying input/output moved onto adapter connections, so the block can be wired into an application entirely through adapters (`AX`, `AS`, `ATM`) without pulling individual Event/BOOL/TIME lines. The timing (`timeOut`) stays internal and is not exposed.

## Function Blocks Used (FBs)

### Sub-modules: AnlagenSequenz_06_ADAPTER

- **Type**: Composite (FBNetwork)
- **Internal FBs used**:
    - **Sequence**: `logiBUS::utils::sequence::timed::AnlagenSequenz_06` - the actual ring sequencer, see [AnlagenSequenz_06](AnlagenSequenz_06.md) for the full state overview.
    - **TimeTicker**: `adapter::events::TimeOut::TimeTicker` - generates the cyclic time tick (`TC`, default `T#200ms`) for the internal timing.
    - **E_TimeOut**: `adapter::events::TimeOut::E_FB_TimeOut` - couples `Sequence.timeOut` to `TimeTicker`, so the lead/lag times run without a dedicated timer FB in the application.
- **Functionality**: Bundles the six `DO_Mx` run commands and six `STOERUNG_Mx` fault inputs, plus every status value (`STATUS_BETRIEB`, `STATUS_STOERUNG`, `ZAEHLSTAND`, `EINSCHALTBEREIT`, `TRANSIT_MOTOR`, `TRANSIT_ART`, `ANLAGE_AUS`), into `AX`/`AS` adapters, and feeds each of the ten dwell times (`ZE1..5_EIN`/`_AUS`) through its own `ATM` adapter socket instead of exposing them as individual `TIME` data inputs. `PT`/`ET` (process/elapsed time) are exposed as `ATM` plugs for a readback of the internal timing.

## Program Flow and Connections

1. `EIN`/`AUS` pass through unchanged to `Sequence.EIN`/`Sequence.AUS`.
2. Each `ATM_ZEk_EIN`/`ATM_ZEk_AUS` adapter delivers its time (`.D1`) to `Sequence.ZEk_EIN`/`Sequence.ZEk_AUS` and simultaneously (`.E1`) triggers the matching `Sequence.SET_ZEk_EIN`/`SET_ZEk_AUS` set event.
3. Each `STOERUNG_Mx` adapter delivers its fault signal (`.D1`) to `Sequence.STOERUNG_Mx` and (`.E1`) triggers `Sequence.EI_Mx`.
4. `Sequence.CNF` simultaneously triggers all status adapters (`STATUS_BETRIEB`, `STATUS_STOERUNG`, `ZAEHLSTAND`, `EINSCHALTBEREIT`, `TRANSIT_MOTOR`, `TRANSIT_ART`, `ANLAGE_AUS`); the associated data values run in parallel via their `.D1`.
5. Each `Sequence.EO_Mx` triggers `DO_Mx.E1`, with the run command carried via `DO_Mx.D1`.
6. `Sequence.INITO` starts the `TimeTicker`; its `CNF`/`STARTO` feed `ET`/`PT` for a pure time readback to the outside.
7. `Sequence.timeOut` (adapter socket) is connected internally to `E_TimeOut.TimeOutSocket`, `TimeTicker.TimeTickSocket` to `E_TimeOut.TimeTickSocket` - the entire timing stays encapsulated inside the composite.

## Application Scenarios

- Applications that consistently rely on adapter connections (e.g. for VT integration or OPC-UA exposure via existing adapter conversion chains) and don't want to pull individual event/data lines to the motor sequence.
- Reusing the same ring-sequencer logic across multiple plant sections while keeping the wiring uniform through standardized adapter types (`AX`, `AS`, `ATM`).

## ⚖️ Comparison with Similar Function Blocks

- **[AnlagenSequenz_06](AnlagenSequenz_06.md)**: The underlying basic-FB logic with classic event/data connections. `AnlagenSequenz_06_ADAPTER` only changes the interface, not the state behavior.
- **[sequence_T_08_ADAPTER](sequence_T_08_ADAPTER.md)**: Same adapter-wrapper pattern, applied to the generic 8-step sequencer instead of the fixed 6-motor ring topology.

## Conclusion

`AnlagenSequenz_06_ADAPTER` makes the complete ring-sequencer logic for six motors usable through pure adapter connections, without changing the underlying state logic - the timing stays fully encapsulated internally.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
