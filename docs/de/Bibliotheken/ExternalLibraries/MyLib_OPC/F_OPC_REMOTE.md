# F_OPC_REMOTE: Funktion (IEC 61499 Function)

![F_OPC_REMOTE](./F_OPC_REMOTE.svg)

* * * * * * * * * *

## Einleitung

`F_OPC_REMOTE` ist keine SubApp, sondern ein Funktionstyp (`.fct`, Standard 61499-1) im Paket `MyLib_OPC` mit dem Kommentar "OPC-UA-ID fuer einen Knoten auf einer anderen Station: opc_ua[ACTION;ENDPOINT#;NODE]". Er hat einen Ereigniseingang `REQ`, einen Ereignisausgang `CNF` und einen Strukturierter-Text-Rumpf; es gibt keine Funktionsbaustein-Instanzen und keine Verbindungen.

## Schnittstelle der Funktion

| Instanz | Typ | Parameter | Erklärung |
|---|---|---|---|
| `REQ` | `Event (Ereigniseingang)` | `With` = `sAction`, `sEndpoint`, `sNode` | Anforderung zur Ausführung. |
| `CNF` | `Event (Ereignisausgang)` | `With` = (unbenannte Variable) | Bestätigung; liefert den Rückgabewert. |
| `sAction` | `WSTRING (Eingangsvariable)` | - | ACTION, z.B. `WRITE` oder `CALL_METHOD` |
| `sEndpoint` | `WSTRING (Eingangsvariable)` | - | Endpunkt, z.B. `opc.tcp://192.168.1.12:4840` |
| `sNode` | `WSTRING (Eingangsvariable)` | - | Knoten, z.B. `/Objects/X/Y,1:s=Y` |
| (unbenannt) | `WSTRING (Ausgangsvariable)` | - | Rückgabewert `F_OPC_REMOTE` (leerer Ausgangsname). |

## Programmablauf und Verbindungen

Der Rumpf ist in Structured Text geschrieben:

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

1. `CONCAT` setzt `opc_ua[`, `sAction`, `;`, `sEndpoint`, `#;`, `sNode` und `]` zusammen; das `#` schließt den Endpunkt ab.

## Beispiel

Aufruf und Ergebnis:

```
MyLib_OPC::F_OPC_REMOTE("WRITE", "opc.tcp://192.168.1.12:4840", "/Objects/X/Y,1:s=Y")  ->  opc_ua[WRITE;opc.tcp://192.168.1.12:4840#;/Objects/X/Y,1:s=Y]
```

## Anwendung

Verwendet in `myOpcUaAddresses.gcf` (Projekt `test_VV`, Ordner `const/OPC_UA`, beschrieben in `OPC_UA_Adressen.md`) als `InitialValue` einer WSTRING-Konstante:

```xml
InitialValue="MyLib_OPC::F_OPC_REMOTE(&quot;WRITE&quot;, PC_B_OPC, Q1_NODE)"
```

`Q1_REMOTE_WRITE` (Station A schreibt auf Station B) nutzt `MyLib_OPC::F_OPC_REMOTE("WRITE", PC_B_OPC, Q1_NODE)`; der Endpunkt `PC_B_OPC` stammt aus `F_MGR_TO_OPC`.

Der Ausdruck wird beim Export ausgewertet; im Bootfile stehen daher nur die fertigen Strings, keine Funktionsaufrufe.

**Hinweis:** Die Argumente müssen Konstanten sein (Literale oder andere Konstanten der `.gcf`), da die Auswertung zur Exportzeit erfolgt und nicht zur Laufzeit.

## Zusammenfassung

`F_OPC_REMOTE` ist eine kleine IEC-61499-Funktion der Bibliothek `MyLib_OPC`, die die OPC-UA-ID für einen Knoten auf einer anderen Station zusammensetzt.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
