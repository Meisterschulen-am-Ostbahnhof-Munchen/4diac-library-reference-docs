# init_stages

## Introduction

The library `init_stages` (package `events::init`) provides blocks for **staged initialization**. Background: remote OPC UA clients wait up to 60 s per missing node at startup if the server side is initialized too late. Stages let you define the order at startup.

## Function Blocks

| Block | Event type | Purpose |
| ----- | ---------- | ------- |
| [`INIT0`](events/init/INIT0.md) | `E0Init` | Stage 0, before `INIT` |
| [`INIT1`](events/init/INIT1.md) | `E1Init` | Stage 1 |
| [`INIT`](events/init/INIT.md) | `EInit` | Standard stage |
| [`INIT2`](events/init/INIT2.md) | `E2Init` | Stage 2, after `INIT` |
| [`INIT3`](events/init/INIT3.md) | `E3Init` | Stage 3, after stage 2 |
| [`INIT_ON`](events/init/INIT_ON.md), [`INIT_OFF`](events/init/INIT_OFF.md), [`INIT_ON_OFF`](events/init/INIT_ON_OFF.md) | `EInitOn`, `EInitOff` | additional channels for the on/off path |

The blocks `INIT` and the `INIT_ON` family used to be in `iec61131-3-bool` (package `iec61131::booleanOperators`) and have moved into this library.

## Principle

- [`E_TRIG`](../../StandardLibraries/events/E_TRIG.md) triggers all **unconnected** inputs of an event type (`EVENTTYPE`) in a resource.
- The order within one type is **random**: it follows the addresses of the StringIds. A fixed order exists only between types.
- Therefore several `E_TRIG` blocks are chained in `EMB_RES`, one per stage: `START.COLD` → `E_TRIG0` (`'E0Init'`) → `E_TRIG` (`'EInit'`) → `E_TRIG2` (`'E2Init'`) → `E_TRIG3` (`'E3Init'`). An `E1Init` stage is possible.
- `INITn` is placed directly next to its target, `INITnO` is connected to its `INIT`. Nothing has to be wired through the hierarchy.
- A stage counts as done when everything has been **triggered**, not when everything has **finished**.

## Related Exercises

- [Uebung_006g_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006g_AX/): SR flip-flop via `INIT_ON_OFF`, triggered by two `E_TRIG` (model for event types via `E_TRIG`)
