# OptionalGroup_001b

![OptionalGroup_001b_network](./OptionalGroup_001b_network.svg)

* * * * * * * * * *

## Introduction

`OptionalGroup_001b` is a SubApp (library `MyLib_AX_Option`): option group 1b, whose digital input `Input_I1` and digital output `Output_Q1` (logiBUS) are enabled via the option adapter. The `QI` from [`AOption_X_TO_EVENT`](../../Option/conversion/AOption_X_TO_EVENT.md) activates or deactivates the whole group. It is the last link of a daisy chain and passes nothing on.

![OptionalGroup_001b](OptionalGroup_001b.svg)

## Used Function Blocks (FBs)

### Sub-blocks: OptionalGroup_001b

- **Type**: SubAppType
- **Interface**: socket `AOption_IN` of type `Option::AOption`.
- **Internal FBs used**:
    - **AOption_X_TO_EVENT**: `Option::conversion::AOption_X_TO_EVENT`, converts the adapter into `INIT_REQ`/`QI_OUT`/`INIT_CNF`.
    - **DigitalInput_I1**: `logiBUS::io::DI::logiBUS_IXA`, parameter `Input = Input_I1`.
    - **DigitalOutput_Q1**: `logiBUS::io::DQ::logiBUS_QXA`, parameter `Output = Output_Q1`.

## Program Flow and Connections

- `AOption_IN` -> `AOption_X_TO_EVENT.AOption_IN`
- Init chain: `AOption_X_TO_EVENT.INIT_REQ` -> `DigitalInput_I1.INIT`; `DigitalInput_I1.INITO` -> `DigitalOutput_Q1.INIT`; `DigitalOutput_Q1.INITO` -> `AOption_X_TO_EVENT.INIT_CNF`
- `AOption_X_TO_EVENT.QI_OUT` -> `DigitalInput_I1.QI` and `DigitalOutput_Q1.QI`
- Adapter connection `DigitalInput_I1.IN` -> `DigitalOutput_Q1.OUT`: the input is routed through to the output.

## Technical Features

- Identical in construction to [`OptionalGroup_001a`](OptionalGroup_001a.md), but without the plug `AOption_OUT`.
- The initialization (input, then output) is only acknowledged to the socket once both blocks are done.

## Application Scenarios

- Typically attached to `AOption_OUT` of an upstream [`OptionalGroup_001a`](OptionalGroup_001a.md), so both groups are activated one after the other (first a, then b) before the original caller receives its acknowledgment.

## Summary

`OptionalGroup_001b` bundles `Input_I1` and `Output_Q1` into an option group switchable via `AOption` and forms the end of a daisy chain.

## 🛠️ Related Exercises

- [Uebung_094b3c_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3c_AX/)
