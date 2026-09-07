# E_SR

## 🎧 Podcast

![E_SR_ecc](./E_SR_ecc.svg)

- [IEC 61499: The E_SR Function Block Decoded – Simplicity Meets Event Control ](https://podcasters.spotify.com/pod/show/iec-61499-grundkurs-de/episodes/IEC-61499-Der-E_SR-Baustein-entschlsselt--Einfachheit-trifft-Ereignissteuerung-e3682bo)
- [Decoding the E_SR Function Block: The Unsung Hero of Industrial Automation ](https://podcasters.spotify.com/pod/show/iec-61499-prime-course-en/episodes/Decoding-the-E_SR-Function-Block-The-Unsung-Hero-of-Industrial-Automation-e3681qo)

## Introduction

The `E_SR` (Event-driven SR Flip-Flop) is an event-driven, bistable function block according to IEC 61499. It serves as a basic memory element controlled by separate "Set" and "Reset" events. Its output, `Q`, retains its state until an opposing event occurs.

![E_SR](E_SR.svg)

## Interface Structure

### **Event Inputs:**

- **S (Set)**: Sets the output `Q` to `TRUE`.
- **R (Reset)**: Sets the output `Q` to `FALSE`.

### **Event Outputs:**

- **EO (Event Output)**: Triggered when the state of `Q` changes.
- **Associated Data**: `Q`

### **Data Outputs:**

- **Q**: The current state of the flip-flop (data type: `BOOL`).

## Functionality

The `E_SR` block functions as a simple latch:

1. **Set**: When an event arrives at the input `S`, the output `Q` is set to `TRUE`. If `Q` was previously `FALSE`, the `EO` event is triggered.
2. **Reset**: When an event arrives at the input `R`, the output `Q` is set to `FALSE`. If `Q` was previously `TRUE`, the `EO` event is triggered.
3. **Save**: Between events, `Q` retains its last set state.

## Technical Features and Standards Comparison

According to **DIN EN 61499-1 (Table A.1, Note 8)**, the implementation of this function block is identical to [E_RS](E_RS.md). Both function blocks (`E_SR` and `E_RS`) exist to maintain consistency with the types in IEC 61131-3, even though IEC 61499 does not have an inherent "dominance" of events, as is the case with level-controlled inputs in classic PLC programming.

- **Comparison to IEC 61131-3**: See [SR (Bistable, set first)](../../Vergleich/IEC61131_3/SR_ALT.md). While in IEC 61131-3 the `SR` function block has a defined "set dominance" (if S and R are TRUE simultaneously, S wins), in IEC 61499 the behavior with closely spaced events depends on the processing order of the runtime environment (ECC). Since events are transient, there is no permanent conflict between two static signals.
- **Functional Identity**: `E_SR` and `E_RS` are technically identical. Their graphical representation and naming conventions simply follow established naming conventions to aid developers.
- **Change Detection**: The `EO` output is only triggered by an actual state change.

## Application Scenarios

- **Start/Stop Logic**: A "Start" button is connected to `S`, and a "Stop" button to `R`, to control the state of a machine.
- **Start/Stop Logic**: - **Error Storage**: An error event sets the function block (`S`), which stores the error state until it is explicitly acknowledged by an operator or another process (`R`).
- **Mode Storage**: Stores the current operating mode of a system (e.g., "Manual" vs. "Automatic").

## Related Function Blocks

- **[E_RS](E_RS.md)**: Functionally identical to `E_SR`. The only difference is the graphical arrangement of the `S` and `R` connections on the symbol.
- **`E_D_FF`**: Also stores a state, but on a clock-based basis. `E_D_FF` takes the value from the `D` input when a `CLK` event occurs.

E_D_FF`

## 🛠️ Related exercises

- [Uebung_004b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b/)
- [Uebung_004b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b2/)
- [Uebung_004b3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b3/)
- [Uebung_006](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006/)
- [Uebung_006c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006c/)
- [Uebung_006d](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006d/)
- [Uebung_007a3](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a3/)
- [Uebung_008](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_008/)
- [Uebung_009](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_009/)
- [Uebung_013](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_013/)
- [Uebung_014](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_014/)
- [Uebung_015](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_015/)
- [Uebung_016](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_016/)
- [Uebung_019b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019b/)
- [Uebung_019c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019c/)
- [Uebung_021](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_021/)
- [Uebung_022](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_022/)
- [Uebung_023](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_023/)
- [Uebung_024](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_024/)
- [Exercise_025](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_025/)
- [Exercise_026_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_026_sub/)
- [Exercise_039a_sub_Outputs](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a_sub_Outputs/)
- [Exercise_160b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b/)

## Conclusion

The `E_SR` block is a fundamental memory block in IEC 61499. It is ideal for simple state storage where a state is set by one event and explicitly reset by another. The lack of guaranteed set or reset dominance for simultaneous events must be considered in critical applications.
