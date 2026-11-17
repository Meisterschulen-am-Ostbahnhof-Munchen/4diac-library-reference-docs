# NmGetCfInfo

* * * * * * * * * *

## Introduction

The function block `NmGetCfInfo` is used within an ISOBUS network to retrieve information about connected communication partners (Connected CFs). It allows both internal and external participants in the network to be identified and their details to be recorded. This block is part of ISOBUS PGN communication and is typically used for network management tasks.
![NmGetCfInfo](NmGetCfInfo.svg)

## Interface Structure

### **Event Inputs**

- **INIT (Type: EInit)**: Initializes the service. When triggered, the configured data inputs are accepted and the initialization process is started.
- **RSP (Type: Event)**: Acknowledges receipt of an indication (`IND`). This event signals to the module that a previously sent indication has been processed.

### **Event Outputs**

- **INITO (Type: EInit)**: Confirms successful completion of initialization.
- **IND (Type: Event)**: Triggered to forward a received network indication (e.g., detected communication partners) to the application.

### **Data Inputs**

- **u8CanIdx (Type: USINT, Initial Value: ISO_CAN_NODE::INVALID)**: Filter on the CAN node (CAN controller index). `INVALID` means all CAN nodes.
- **member (Type: SINT, Initial Value: ISOUSERHOME_e::notdef)**: Filter on the kind of participant (internal or external, see `eMemberRefer`). `notdef` means all.
- **address (Type: isobus::pgn::NAMEFIELD_T)**: Filter value for the ISOBUS name (NAME) of the participants. Only the bits that are set in `mask` are compared. It is not an address for which information is retrieved.
- **mask (Type: isobus::pgn::NAMEFIELD_T)**: Bit mask for `address`. Only set bits (1) are compared. With the default value (all bits 0) every participant matches.

### **Data Outputs**

- **bwaitingForRSP (Type: BOOL)**: `TRUE` from the `IND` until the application answers with `RSP`. While it is `TRUE`, the block outputs no further `IND`.
- **sNetEv (Type: isobus::pgn::ISONETEVENT_T)**: The network event concerning the participant (see section "Network Events"), including `eNetEvent` and `eMemberRefer`.
- **sCfInfo (Type: isobus::pgn::CF_INFO_T)**: The retrieved information about the found communication partner (CF = Communication Function).
- **sNameField (Type: isobus::pgn::NAMEFIELD_T)**: The ISOBUS name of the detected communication partner.

### **Adapter**

This function block does not use any adapter interfaces.

## Functionality

1. **Initialization**: With `INIT` the block takes over the filters (`u8CanIdx`, `member`, `address`, `mask`). It then outputs all already known participants that match the filter once more as event `MemberActive` (replay from a cache in the driver). So the application also sees participants that were on the network before the start. `INITO` follows.
2. **Notification (`IND`)**: The block does not search by itself. The stack reports every change on the network, and the block passes on the reports that match the filter. Each report triggers `IND`, with `sNetEv`, `sCfInfo` and `sNameField`, and sets `bwaitingForRSP` to `TRUE`. This applies to new participants as well as to log-offs, address conflicts and timeouts.
3. **Acknowledgement (`RSP`)**: The application has to acknowledge every `IND` with `RSP` (connect output `IND` to input `RSP`). The block then sets `bwaitingForRSP` to `FALSE` and, if reports are waiting, outputs the next one. **Without `RSP` no further `IND` comes.** All following reports, including log-offs, stay in a queue inside the block, so the application never sees logged-off participants disappear.

## Technical Features

- The function block works with the `IND`/`RSP` pattern: every report has to be acknowledged before the next one comes.
- The use of a bit-based `mask` enables flexible search queries, e.g., for participants of a specific device class.
- The data types (`ISONETEVENT_T`, `CF_INFO_T`, `NAMEFIELD_T`) are specific to ISOBUS PGN communication and contain structured information according to the ISO 11783 standard.
-

## Network Events

`sNetEv.eNetEvent` states what happened. `IND` reports every one of these changes, not only new participants, so the application has to evaluate `eNetEvent`.

| Value | Event | Meaning |
| ---: | --- | --- |
| 0 | `MemberActive` | Participant is active (new on the network, or replay after `INIT`). |
| 1 | `MemberAddressConflict` | Participant has lost an address conflict. |
| 2 | `MemberInactive` | Participant is no longer active: logged off, timeout or could not claim an address. |
| 3 | `MemberNewAddress` | The own participant has claimed a new address after a conflict. |
| 4 | `MemberAddressViolation` | The own participant was violated: an external message arrived with the same source address. |

## Notes on Operation

- **`RSP` is mandatory.** Without `RSP` the block stays in "Waiting for RSP" (see above).
- **Reports even before `INIT`:** The block registers with the driver when it is created. An `IND` can therefore come before `INIT`, then still unfiltered (with the default values of the filters).
- **Duplicate reports are possible:** Together with the replay at `INIT`, the same participant can arrive several times as `MemberActive`. The application has to cope with that.


## Status Overview

1. **Inactive**: Before initialization.
2. **Initialized**: After `INITO`. The function block is ready for operation and can process requests.
3. **Waiting for RSP**: After `IND` is triggered. Further reports are collected in a queue and output only after `RSP`.

## Application Scenarios

- **Network Discovery**: When a device starts up, to identify all active participants in the ISOBUS network.
- **Diagnostics and Monitoring**: To monitor whether specific expected control units (e.g., of an implement) are connected and reachable.
- **Dynamic Configuration**: To obtain information about newly added devices and adapt the application accordingly.

## ⚖️ Comparison with Similar Modules

Unlike simple read or query modules, `NmGetCfInfo` is specifically designed for querying ISOBUS-specific network information. It offers more context (through `member`, `address`, `mask`) and structured result data (`CF_INFO_T`) than a generic communication module. Modules like `E_SWITCH` or `E_DEMUX` only forward events, while `NmGetCfInfo` performs active network communication and protocol handling.

Modules like `E_SWITCH` or `E_DEMUX` only forward events, while `NmGetCfInfo` performs active network communication and protocol execution.

## Cross-References

- **Communication blocks** (for example [`SERVER_0`](../../../StandardLibraries/net/SERVER_0.md) and [`SUBSCRIBE_0`](../../../StandardLibraries/net/SUBSCRIBE_0.md) of the library `net`, also the AX adapter blocks for them): They also have the pair `IND`/`RSP`. The difference: with the subscriber `RSP` has no effect, nothing is sent. With the server `RSP` sends the response. With `NmGetCfInfo`, on the other hand, `RSP` controls the flow, it releases the next report.
- **Queue blocks** (library FIFO, for example `FIFO_UINT`): `NmGetCfInfo` also buffers the reports internally in a queue following the "first in, first out" principle. The buffer is built into the block, but it passes on the next report only after `RSP`.


## 🛠️ Related exercises

- [Uebung_120](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_120/)
- [Uebung_121](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_121/)
- [Uebung_122](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_122/)
- [Uebung_122b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_122b/)
- [Uebung_123](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_123/)
- [Uebung_124](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_124/)
- [Uebung_125](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_125/)
- [Uebung_126](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_126/)
- [Uebung_126b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_126b/)
- [Uebung_126b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_126b2/)
- [Uebung_127](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_127/)
- [Uebung_128](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128/)
- [Uebung_128b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128b/)
- [Uebung_130](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_130/)
- [Uebung_131](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_131/)
- [Uebung_132](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_132/)
- [Uebung_133](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_133/)
- [Uebung_134](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_134/)

## Conclusion

The `NmGetCfInfo` function block is an essential tool for managing ISOBUS networks. It abstracts the complex protocol communication required to detect network participants into an easy-to-use, event-driven interface. It reports not only new but also logged-off participants and address changes, filtered by NAME, CAN node and kind of participant. This makes it valuable for robust and intelligent agricultural control systems.
