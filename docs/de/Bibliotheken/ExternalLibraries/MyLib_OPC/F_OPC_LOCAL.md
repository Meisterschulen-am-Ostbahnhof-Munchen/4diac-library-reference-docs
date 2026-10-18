# F_OPC_LOCAL: Funktion (IEC 61499 Function)

![F_OPC_LOCAL](./F_OPC_LOCAL.svg)

* * * * * * * * * *

## Einleitung

`F_OPC_LOCAL` ist keine SubApp, sondern ein Funktionstyp (`.fct`, Standard 61499-1) im Paket `MyLib_OPC` mit dem Kommentar "OPC-UA-ID fuer einen lokalen Knoten: opc_ua[ACTION;NODE]". Er hat einen Ereigniseingang `REQ`, einen Ereignisausgang `CNF` und einen Strukturierter-Text-Rumpf; es gibt keine Funktionsbaustein-Instanzen und keine Verbindungen.

## Schnittstelle der Funktion

| Instanz | Typ | Parameter | Erklärung |
|---|---|---|---|
| `REQ` | `Event (Ereigniseingang)` | `With` = `sAction`, `sNode` | Anforderung zur Ausführung. |
| `CNF` | `Event (Ereignisausgang)` | `With` = (unbenannte Variable) | Bestätigung; liefert den Rückgabewert. |
| `sAction` | `WSTRING (Eingangsvariable)` | - | ACTION, z.B. `READ` oder `CREATE_METHOD` |
| `sNode` | `WSTRING (Eingangsvariable)` | - | Knoten, z.B. `/Objects/X/Y,1:s=Y` |
| (unbenannt) | `WSTRING (Ausgangsvariable)` | - | Rückgabewert `F_OPC_LOCAL` (leerer Ausgangsname). |

## Programmablauf und Verbindungen

Der Rumpf ist in Structured Text geschrieben:

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

1. `CONCAT` setzt `opc_ua[`, `sAction`, `;`, `sNode` und `]` zusammen.

## Beispiel

Aufruf und Ergebnis:

```
MyLib_OPC::F_OPC_LOCAL("READ", "/Objects/X/Y,1:s=Y")  ->  opc_ua[READ;/Objects/X/Y,1:s=Y]
```

## Anwendung

Verwendet in `myOpcUaAddresses.gcf` (Projekt `test_VV`, Ordner `const/OPC_UA`, beschrieben in `OPC_UA_Adressen.md`) als `InitialValue` einer WSTRING-Konstante:

```xml
InitialValue="MyLib_OPC::F_OPC_LOCAL(&quot;READ&quot;, Q1_NODE)"
```

`Q1_LOCAL_READ` und `FF1_TRIGGER_LOCAL_METHOD` (lokal auf Station B) nutzen `MyLib_OPC::F_OPC_LOCAL("READ", Q1_NODE)` bzw. `MyLib_OPC::F_OPC_LOCAL("CREATE_METHOD", FF1_TRIGGER_NODE)`.

Der Ausdruck wird beim Export ausgewertet; im Bootfile stehen daher nur die fertigen Strings, keine Funktionsaufrufe.

**Hinweis:** Die Argumente müssen Konstanten sein (Literale oder andere Konstanten der `.gcf`), da die Auswertung zur Exportzeit erfolgt und nicht zur Laufzeit.

## Zusammenfassung

`F_OPC_LOCAL` ist eine kleine IEC-61499-Funktion der Bibliothek `MyLib_OPC`, die die OPC-UA-ID für einen lokalen Knoten zusammensetzt.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
