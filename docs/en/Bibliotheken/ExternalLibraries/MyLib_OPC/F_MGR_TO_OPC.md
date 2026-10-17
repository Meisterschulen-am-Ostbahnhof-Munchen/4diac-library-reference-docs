# F_MGR_TO_OPC: Function (IEC 61499 Function)

![F_MGR_TO_OPC](./F_MGR_TO_OPC.svg)

* * * * * * * * * *

## Introduction

`F_MGR_TO_OPC` is not a SubApp but a function type (`.fct`, standard 61499-1) in the package `MyLib_OPC` with the comment "OPC-UA endpoint from MGR_ID: 192.168.1.12:61499 -> opc.tcp://192.168.1.12:4840". It has one event input `REQ`, one event output `CNF` and a Structured Text body; there are no function block instances and no connections.

## Function Interface

| Instance | Type | Parameters | Explanation |
|---|---|---|---|
| `REQ` | `Event (event input)` | `With` = `sMgr` | Execution request. |
| `CNF` | `Event (event output)` | `With` = (unnamed variable) | Confirmation; delivers the return value. |
| `sMgr` | `WSTRING (input variable)` | - | MGR_ID of the station, e.g. `logiBUS::stations::Computers::C_12` |
| (unnamed) | `WSTRING (output variable)` | - | Return value `F_MGR_TO_OPC` (empty output name). |

## Program Flow and Connections

The body is written in Structured Text:

```
PACKAGE MyLib_OPC;

(* OPC-UA-Endpunkt aus MGR_ID: 192.168.1.12:61499 -> opc.tcp://192.168.1.12:4840 *)
FUNCTION F_MGR_TO_OPC : WSTRING

VAR_INPUT
	sMgr : WSTRING; // MGR_ID der Station, z.B. logiBUS::stations::Computers::C_12
END_VAR

F_MGR_TO_OPC := CONCAT("opc.tcp://", LEFT(sMgr, FIND(sMgr, ":")), "4840");

END_FUNCTION
```

1. `FIND(sMgr, ":")` returns the position of the first colon.
2. `LEFT(sMgr, ...)` cuts the text up to and including this colon (`192.168.1.12:`); the MGR port `61499` is dropped.
3. `CONCAT` joins `opc.tcp://`, this part and the fixed OPC-UA port `4840`.

## Example

Call and result:

```
MyLib_OPC::F_MGR_TO_OPC('192.168.1.12:61499')  ->  opc.tcp://192.168.1.12:4840
```

## Usage

Used in `myOpcUaAddresses.gcf` (project `test_VV`, folder `const/OPC_UA`, described in `OPC_UA_Adressen.md`) as the `InitialValue` of a WSTRING constant:

```xml
InitialValue="MyLib_OPC::F_MGR_TO_OPC(PC_A)"
```

`PC_A_OPC` and `PC_B_OPC` get the OPC-UA endpoint of the respective station via `MyLib_OPC::F_MGR_TO_OPC(PC_A)` and `MyLib_OPC::F_MGR_TO_OPC(PC_B)`.

The expression is evaluated at export time; the boot file therefore contains only the finished strings, no function calls.

**Note:** The arguments must be constants (literals or other constants of the `.gcf`), because evaluation happens at export time, not at runtime.

## Summary

`F_MGR_TO_OPC` is a small IEC 61499 function of the library `MyLib_OPC` that derives the OPC-UA endpoint from a MGR_ID.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
