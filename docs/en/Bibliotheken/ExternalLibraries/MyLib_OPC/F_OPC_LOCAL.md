# F_OPC_LOCAL: Function (IEC 61499 Function)

![F_OPC_LOCAL](./F_OPC_LOCAL.svg)

* * * * * * * * * *

## Introduction

`F_OPC_LOCAL` is not a SubApp but a function type (`.fct`, standard 61499-1) in the package `MyLib_OPC` with the comment "OPC-UA ID for a local node: opc_ua[ACTION;NODE]". It has one event input `REQ`, one event output `CNF` and a Structured Text body; there are no function block instances and no connections.

## Function Interface

| Instance | Type | Parameters | Explanation |
|---|---|---|---|
| `REQ` | `Event (event input)` | `With` = `sAction`, `sNode` | Execution request. |
| `CNF` | `Event (event output)` | `With` = (unnamed variable) | Confirmation; delivers the return value. |
| `sAction` | `WSTRING (input variable)` | - | ACTION, e.g. `READ` or `CREATE_METHOD` |
| `sNode` | `WSTRING (input variable)` | - | Node, e.g. `/Objects/X/Y,1:s=Y` |
| (unnamed) | `WSTRING (output variable)` | - | Return value `F_OPC_LOCAL` (empty output name). |

## Program Flow and Connections

The body is written in Structured Text:

```
PACKAGE MyLib_OPC;

(* OPC-UA-ID fuer einen lokalen Knoten: opc_ua[ACTION;NODE] *)
FUNCTION F_OPC_LOCAL : WSTRING

VAR_INPUT
	sAction : WSTRING; // ACTION, z.B. READ oder CREATE_METHOD
	sNode : WSTRING; // Knoten, z.B. /Objects/X/Y,1:s=Y
END_VAR

F_OPC_LOCAL := CONCAT("opc_ua[", sAction, ";", sNode, "]");

END_FUNCTION
```

1. `CONCAT` joins `opc_ua[`, `sAction`, `;`, `sNode` and `]`.

## Example

Call and result:

```
MyLib_OPC::F_OPC_LOCAL("READ", "/Objects/X/Y,1:s=Y")  ->  opc_ua[READ;/Objects/X/Y,1:s=Y]
```

## Usage

Used in `myOpcUaAddresses.gcf` (project `test_VV`, folder `const/OPC_UA`, described in `OPC_UA_Adressen.md`) as the `InitialValue` of a WSTRING constant:

```xml
InitialValue="MyLib_OPC::F_OPC_LOCAL(&quot;READ&quot;, Q1_NODE)"
```

`Q1_LOCAL_READ` and `FF1_TRIGGER_LOCAL_METHOD` (local on station B) use `MyLib_OPC::F_OPC_LOCAL("READ", Q1_NODE)` and `MyLib_OPC::F_OPC_LOCAL("CREATE_METHOD", FF1_TRIGGER_NODE)`.

The expression is evaluated at export time; the boot file therefore contains only the finished strings, no function calls.

**Note:** The arguments must be constants (literals or other constants of the `.gcf`), because evaluation happens at export time, not at runtime.

## Summary

`F_OPC_LOCAL` is a small IEC 61499 function of the library `MyLib_OPC` that assembles the OPC-UA ID for a local node.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
