# F_MGR_TO_OPC: Funktion (IEC 61499 Function)

![F_MGR_TO_OPC](./F_MGR_TO_OPC.svg)

* * * * * * * * * *

## Einleitung

`F_MGR_TO_OPC` ist keine SubApp, sondern ein Funktionstyp (`.fct`, Standard 61499-1) im Paket `MyLib_OPC` mit dem Kommentar "OPC-UA-Endpunkt aus MGR_ID: 192.168.1.12:61499 -> opc.tcp://192.168.1.12:4840". Er hat einen Ereigniseingang `REQ`, einen Ereignisausgang `CNF` und einen Strukturierter-Text-Rumpf; es gibt keine Funktionsbaustein-Instanzen und keine Verbindungen.

## Schnittstelle der Funktion

| Instanz | Typ | Parameter | Erklärung |
|---|---|---|---|
| `REQ` | `Event (Ereigniseingang)` | `With` = `sMgr` | Anforderung zur Ausführung. |
| `CNF` | `Event (Ereignisausgang)` | `With` = (unbenannte Variable) | Bestätigung; liefert den Rückgabewert. |
| `sMgr` | `WSTRING (Eingangsvariable)` | - | MGR_ID der Station, z.B. `logiBUS::stations::Computers::C_12` |
| (unbenannt) | `WSTRING (Ausgangsvariable)` | - | Rückgabewert `F_MGR_TO_OPC` (leerer Ausgangsname). |

## Programmablauf und Verbindungen

Der Rumpf ist in Structured Text geschrieben:

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

1. `FIND(sMgr, ":")` liefert die Position des ersten Doppelpunkts.
2. `LEFT(sMgr, ...)` schneidet den Text bis einschließlich dieses Doppelpunkts ab (`192.168.1.12:`); der MGR-Port `61499` entfällt.
3. `CONCAT` setzt `opc.tcp://`, diesen Teil und den festen OPC-UA-Port `4840` zusammen.

## Beispiel

Aufruf und Ergebnis:

```
MyLib_OPC::F_MGR_TO_OPC('192.168.1.12:61499')  ->  opc.tcp://192.168.1.12:4840
```

## Anwendung

Verwendet in `myOpcUaAddresses.gcf` (Projekt `test_VV`, Ordner `const/OPC_UA`, beschrieben in `OPC_UA_Adressen.md`) als `InitialValue` einer WSTRING-Konstante:

```xml
InitialValue="MyLib_OPC::F_MGR_TO_OPC(PC_A)"
```

`PC_A_OPC` und `PC_B_OPC` erhalten den OPC-UA-Endpunkt der jeweiligen Station über `MyLib_OPC::F_MGR_TO_OPC(PC_A)` bzw. `MyLib_OPC::F_MGR_TO_OPC(PC_B)`.

Der Ausdruck wird beim Export ausgewertet; im Bootfile stehen daher nur die fertigen Strings, keine Funktionsaufrufe.

**Hinweis:** Die Argumente müssen Konstanten sein (Literale oder andere Konstanten der `.gcf`), da die Auswertung zur Exportzeit erfolgt und nicht zur Laufzeit.

## Zusammenfassung

`F_MGR_TO_OPC` ist eine kleine IEC-61499-Funktion der Bibliothek `MyLib_OPC`, die den OPC-UA-Endpunkt aus einer MGR_ID ableitet.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
