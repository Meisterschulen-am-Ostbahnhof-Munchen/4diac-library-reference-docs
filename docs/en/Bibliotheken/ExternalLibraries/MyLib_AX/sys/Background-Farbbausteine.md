# Background Color Blocks: Common Pattern

* * * * * * * * * *

## Introduction

In both training systems (`test_AX` and `test_B`), `MyLib::sys` contains a large family of nearly identical blocks that **toggle the VT background color of one or more objects based on a single Boolean signal** — e.g., `GreenWhiteBackground1_AX`, `GreenRedBackground4_AXS`, `RedWhiteBackground2_AXC`. This page explains the common pattern in full; the individual block pages refer back to this page and only mention the specific differences (color pair, number of objects, variant).

## Naming Scheme

`<Farbe1><Farbe2>Background<N>[_aux][_AX][S][C]`

| Component | Meaning |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Color Pair** (`GreenWhite`, `GreenRed`, `GreenBlue`, `RedGreen`, `RedWhite`) | `TRUE` → first color, `FALSE` → second color (e.g., `GreenWhiteBackground`: TRUE→Green, FALSE→White) |
| **N** (1–4) | Number of VT objects that are **simultaneously colored from the same selector bit** (not: number of independent channels) |
| **`_aux`** (only for N=1) | Uses `Q_BackgroundColourAux` instead of `Q_BackgroundColour` — targets an ISOBUS **Auxiliary Function** object instead of a normal softkey/button/data mask object. For N≥2 variants, this role may already be permanently integrated into one of the positions (see individual page). |
| **`_AX`** (only for test_AX) | The selector signal arrives via a `AX` adapter socket (`DI1`), not as a pure data input. This suffix is missing in test_B — there, `DI1` is a simple `BOOL` data input. |
| **`S`** | Object ID(s) are passed via the structured type `isobus::UT::Q::types::s1ObjectID` (`u16ObjIds`, internally unpacked via `F_MOVE`) instead of via individual `UINT u16ObjId` inputs — a later standardization of object ID passing in the library. |
| **`C`** ("Compact") | A thin wrapper that instantiates only the base variant (`_AX` or `_AXS`) and does not expose the `CNF` event outputs/intermediate values (`STATUS_n`, `u8OldColour_n`, `result_n`) – for the typical case where these diagnostic values are not needed. |



 Examples: `GreenWhiteBackground1_AX` (Basic, 1 object, adapter), `GreenWhiteBackground1_AXC` (Compact wrapper of this), `GreenWhiteBackground1_AXS` (with struct object ID), `GreenWhiteBackground1_AXSC` (struct ID + compact), `GreenWhiteBackground1_aux_AX` (like Basic, but with an AUX function object).


## Functionality (Basic Variant, N=1)

1. The selector signal (`DI1` adapter for `_AX`, otherwise `DI1` data value) goes to `AX_SEL`/`F_SEL` (binary selection), parameterized with the two color constants (`IN0` = second color, `IN1` = first color, e.g., `IN0=COLOR_WHITE`, `IN1=COLOR_GREEN`).

2. `AX_SEL.CNF` triggers `Q_BackgroundColour.REQ` (or `Q_BackgroundColourAux.REQ` for `_aux`).

3. `Q_BackgroundColour` sets the background color of the VT object identified by `u16ObjId` and returns `CNF` with `STATUS`, `u8OldColour` (previous color), and `s16result` (error code).


## Functionality (N≥2)

For multiple objects (`GreenRedBackground4_AX` etc.), there is still only **one** selector (`DI1`/`AX_SEL`), whose output is distributed in parallel to multiple `Q_BackgroundColour_n` instances (numbered `_1`..`_N`) — each with its own `u16ObjId`/`u16ObjIdA`/`u16ObjIdB` (different object IDs, sometimes for different object roles such as softkey/AUX/button) and its own `STATUS_n`/`u8OldColour_n`/`result_n`/`CNF_n` outputs. **Which position uses `Q_BackgroundColourAux` instead of `Q_BackgroundColour` varies per block** (not always the same position) — the individual page for each block specifies the exact assignment.

## Technical Features

- **One selector, multiple Objectives**: The purpose of N>1 is to apply the same Boolean condition (e.g., "Channel active") simultaneously to multiple VT representations of the same logical state (e.g., softkey background AND auxiliary function background AND button background of the same function).

- **`_aux`/`Q_BackgroundColourAux`**: ISOBUS auxiliary function objects (controls that can be freely assigned to the driver) require their own Q block because they use a different ObjectID range/VT message than normal softkeys/buttons.

- **`S` variant as a later unification**: Recognizable by the later `VersionInfo` dates (2026 vs. 2022) — the `S` variants are a follow-up refactoring that aligns object ID passing with a later-established standard (`s1ObjectID`), without replacing the older base variants.

## Family Overview

| Color Pair | test_AX (Base/_aux, also S/SC/C) | test_B (Base/_aux, also S/SC/C) |
| ----------- | ----------------------------------------- | ---------------------------------------- |
| Green/White | 1, 2, 3, 4 | 1, 2, 3, 4 |
| Green/Red | 1, 2, 3, 4 | 1, 2, 3, 4 |
| Green/Blue | 1 | 1 |
| Red/Green | 1, 4 | 1, 4 |
| Red/White | 1, 2, 3, 4 | 1, 2, 3, 4 |

In test_AX, each combination also carries the suffix `_AX`; in test_B, this suffix is omitted (selector as `BOOL` data input instead of adapter socket) — see [MyLib (test_B) → sys](../../MyLib_B/sys/index.md).

## Summary

The background color blocks are based on a single, recurring concept (Boolean signal → color selection → `Q_BackgroundColour`) with many minor variations for different color pairs, object counts, and object ID conventions. Understanding one variation makes them all understandable—the individual pages simply list the specific parameters.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
