# F_OPC_REMOTE: Function (IEC 61499 Function)

![F_OPC_REMOTE](./F_OPC_REMOTE.svg)

* * * * * * * * * *

## Introduction

`F_OPC_REMOTE` is not a SubApp but a function type (`.fct`, standard 61499-1) in the package `MyLib_OPC` with the comment "OPC-UA ID for a node on another station: opc_ua[ACTION;ENDPOINT#;NODE]". It has one event input `REQ`, one event output `CNF` and a Structured Text body; there are no function block instances and no connections.

## Function Interface

| Instance | Type | Parameters | Explanation |
|---|---|---|---|
| `REQ` | `Event (event input)` | `With` = `sAction`, `sEndpoint`, `sNode` | Execution request. |
| `CNF` | `Event (event output)` | `With` = (unnamed variable) | Confirmation; delivers the return value. |
| `sAction` | `WSTRING (input variable)` | - | ACTION, e.g. `WRITE` or `CALL_METHOD` |
| `sEndpoint` | `WSTRING (input variable)` | - | Endpoint, e.g. `opc.tcp://192.168.1.12:4840` |
| `sNode` | `WSTRING (input variable)` | - | Node, e.g. `/Objects/X/Y,1:s=Y` |
| (unnamed) | `WSTRING (output variable)` | - | Return value `F_OPC_REMOTE` (empty output name). |

## Program Flow and Connections

The body is written in Structured Text:

```
PACKAGE MyLib_OPC;

(* OPC-UA-ID fuer einen Knoten auf einer anderen Station: opc_ua[ACTION;ENDPOINT#;NODE] *)
FUNCTION F_OPC_REMOTE : WSTRING

VAR_INPUT
	sAction : WSTRING; // ACTION, z.B. WRITE oder CALL_METHOD
	sEndpoint : WSTRING; // Endpunkt, z.B. opc.tcp://192.168.1.12:4840
	sNode : WSTRING; // Knoten, z.B. /Objects/X/Y,1:s=Y
END_VAR

F_OPC_REMOTE := CONCAT("opc_ua[", sAction, ";", sEndpoint, "#;", sNode, "]");

END_FUNCTION
```

1. `CONCAT` joins `opc_ua[`, `sAction`, `;`, `sEndpoint`, `#;`, `sNode` and `]`; the `#` terminates the endpoint.

## Example

Call and result:

```
MyLib_OPC::F_OPC_REMOTE("WRITE", "opc.tcp://192.168.1.12:4840", "/Objects/X/Y,1:s=Y")  ->  opc_ua[WRITE;opc.tcp://192.168.1.12:4840#;/Objects/X/Y,1:s=Y]
```

## Usage

Used in `myOpcUaAddresses.gcf` (project `test_VV`, folder `const/OPC_UA`, described in `OPC_UA_Adressen.md`) as the `InitialValue` of a WSTRING constant:

```xml
InitialValue="MyLib_OPC::F_OPC_REMOTE(&quot;WRITE&quot;, PC_B_OPC, Q1_NODE)"
```

`Q1_REMOTE_WRITE` (station A writes to station B) uses `MyLib_OPC::F_OPC_REMOTE("WRITE", PC_B_OPC, Q1_NODE)`; the endpoint `PC_B_OPC` comes from `F_MGR_TO_OPC`.

The expression is evaluated at export time; the boot file therefore contains only the finished strings, no function calls.

**Note:** The arguments must be constants (literals or other constants of the `.gcf`), because evaluation happens at export time, not at runtime.

## Summary

`F_OPC_REMOTE` is a small IEC 61499 function of the library `MyLib_OPC` that assembles the OPC-UA ID for a node on another station.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
