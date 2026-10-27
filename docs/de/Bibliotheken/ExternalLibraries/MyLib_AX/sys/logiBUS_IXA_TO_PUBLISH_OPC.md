# logiBUS_IXA_TO_PUBLISH_OPC

![logiBUS_IXA_TO_PUBLISH_OPC_network](./logiBUS_IXA_TO_PUBLISH_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_IXA_TO_PUBLISH_OPC` liest einen digitalen Eingang (`logiBUS_IXA`) und schreibt den Wert als lokalen OPC-UA-Write (`ACTION=WRITE` ohne Endpunkt) per `AX_PUBLISH_1`. Im Gegensatz zu [`logiBUS_IXA_TO_CLIENT_OPC`](./logiBUS_IXA_TO_CLIENT_OPC.md), das den Wert per `AX_CLIENT_1_0` aktiv auf ein anderes Gerät schreibt, bleibt der Wert hier lokal (kein Endpunkt).

![logiBUS_IXA_TO_PUBLISH_OPC](logiBUS_IXA_TO_PUBLISH_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT` (`EInit`): Initialization Request

### **Ereignis-Ausgänge**

- `INITO` (`EInit`): Initialization Confirm

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `QI` | `BOOL` | Quality Input - TRUE, um den Dienst freizugeben. Initialwert `TRUE`. |
| `Input` | `logiBUS::io::DI::logiBUS_DI_S` | Identifiziert den Eingang `Input_I1..I8`. Initialwert `logiBUS_DI::Invalid`. |
| `ID` | `WSTRING` | Lokale Knotenadresse (`BOOL`, `ACTION=WRITE`, ohne Endpunkt). |

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde. |

### **Adapter**

Keine vorhanden.

## Verwendete Funktionsbausteine (FBs)

- **logiBUS_IXA** (`logiBUS::io::DI::logiBUS_IXA`): physischer digitaler Eingang, identifiziert über `Input_I1..I8`.
- **AX_PUBLISH_1** (`adapter::net::AX_PUBLISH_1`): schreibt den Adapterwert lokal auf die Knotenadresse `ID`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): verknüpft `logiBUS_IXA.QO` und `AX_PUBLISH_1.QO` zu `QO`.

## Programmablauf und Verbindungen

1. Initialisierung: `INIT` → `logiBUS_IXA.INIT`; `logiBUS_IXA.INITO` → `logiBUS_IXA.REQ` und → `AX_PUBLISH_1.INIT`; `AX_PUBLISH_1.INITO` → `INITO` und → `AND_QO.REQ`.
2. Wert: `logiBUS_IXA.IN` → `AX_PUBLISH_1.IN`.
3. Daten: `Input` → `logiBUS_IXA.Input`; `ID` → `AX_PUBLISH_1.ID`.
4. Qualifier: `QI` → `logiBUS_IXA.QI` und `AX_PUBLISH_1.QI`.
5. Qualitätsauswertung: `logiBUS_IXA.QO` → `AND_QO.IN1`; `AX_PUBLISH_1.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Technische Besonderheiten

- Lokales Gegenstück zu [`logiBUS_IXA_TO_CLIENT_OPC`](./logiBUS_IXA_TO_CLIENT_OPC.md): `AX_PUBLISH_1` statt `AX_CLIENT_1_0`, daher keine Endpunkt-Angabe in `ID`.

## Anwendungsszenarien

- Digitalen Eingang als lokalen OPC-UA-Knoten bereitstellen (lokale Übungen auf einem Gerät).

## Zusammenfassung

Ein-Baustein-Brücke von einem physischen digitalen Eingang zu einem lokalen OPC-UA-Write per `AX_PUBLISH_1` - lokales Gegenstück zu [`logiBUS_IXA_TO_CLIENT_OPC`](./logiBUS_IXA_TO_CLIENT_OPC.md).

## 🛠️ Zugehörige Übungen

- [Uebung_OPC_L01_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L01_AX/)
- [Uebung_OPC_L10_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L10_AX/)
- [Uebung_OPC_L13_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L13_AX/)
- [Uebung_OPC_L19_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/OPC_UA_Lokal/Uebung_OPC_L19_AX/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
