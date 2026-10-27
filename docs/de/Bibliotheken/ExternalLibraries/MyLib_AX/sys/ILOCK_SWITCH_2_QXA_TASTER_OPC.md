# ILOCK_SWITCH_2_QXA_TASTER_OPC

![ILOCK_SWITCH_2_QXA_TASTER_OPC_network](./ILOCK_SWITCH_2_QXA_TASTER_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`ILOCK_SWITCH_2_QXA_TASTER_OPC` ist eine Kopie von [`ILOCK_SWITCH_2_QXA_OPC`](./ILOCK_SWITCH_2_QXA_OPC.md) (v1.3, das Original bleibt unverändert) mit einem dritten Kommando-Eingang je Richtung (z. B. Hardware-Taster einer anderen Station oder Remote-Subscribe). Test-Kommando, Funktions-Kommando und Taster werden per `AX_OR_3` zusammengeführt. Wie das Original ist die SubApp ein 2-Richtungs-Doppelwirkungs-Ausgang (Last-Wins mit Schutzzeit): Je Richtung werden das echte Funktions-Kommando (Remote-Subscribe) und das bestehende IO-Test-Kommando (Remote-Subscribe) zusammengeführt, bevor sie in den Interlock gehen. Der IO-Test bleibt nutzbar, kann aber die beiden Richtungen nicht mehr dauerhaft gleichzeitig schalten. Der interlockte Ausgang treibt den physischen logiBUS-Kanal und meldet seinen tatsächlichen Zustand sowohl an die bestehende IO-Test-Überwachung als auch an die echte Funktions-Rückmeldung (z. B. `GreenWhiteBackground` am SoftKey) zurück. Der Baustein ist generisch für jeden „digital DW“-Aktor (siehe `IO-Liste_Vereinheitlicht.md`) mit Bedienung von einem Modul ohne eigenes VT. Erstnutzung: Reihenverstellung KVE16 (Taster S1.7/S1.8 an STG4 I4/I5, Ventil an STG3 Q5/Q6).

![ILOCK_SWITCH_2_QXA_TASTER_OPC](ILOCK_SWITCH_2_QXA_TASTER_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT` (`EInit`): Initialisierung - vom Aufrufer sequenziert statt per Auto-Verteilung, damit `QI` vor der physischen Kanalbindung feststeht.

### **Ereignis-Ausgänge**

- `INITO` (`EInit`): Initialisierung bestätigt (von `DigitalOutput_UP.INITO`) - für Aufrufer, die eine echte Confirm-Kette brauchen (z. B. hinter einem `Option::AOption`-Adapter).

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `Output_UP` | `logiBUS::io::DQ::logiBUS_DO_S` | Physischer Ausgang Richtung UP/Links. Initialwert `logiBUS_DO::Invalid`. |
| `Output_DOWN` | `logiBUS::io::DQ::logiBUS_DO_S` | Physischer Ausgang Richtung DOWN/Rechts. Initialwert `logiBUS_DO::Invalid`. |
| `DT_PROTECT` | `TIME` | Schutz-Totzeit vor Richtungswechsel. Initialwert `T#300ms`. |
| `ID_TEST_READ_UP` | `WSTRING` | Bestehende IO-Test-Subscribe-Adresse UP (z. B. `STG3_Q0x_READ`). |
| `ID_TEST_WRITE_UP` | `WSTRING` | Bestehende IO-Test-Publish-Adresse UP (z. B. `STG3_Q0x_WRITE`). |
| `ID_TEST_READ_DOWN` | `WSTRING` | Bestehende IO-Test-Subscribe-Adresse DOWN. |
| `ID_TEST_WRITE_DOWN` | `WSTRING` | Bestehende IO-Test-Publish-Adresse DOWN. |
| `ID_READ_UP` | `WSTRING` | Echtes Funktions-Kommando UP (Subscribe). |
| `ID_WRITE_UP` | `WSTRING` | Echte Funktions-Rückmeldung UP (Publish). |
| `ID_READ_DOWN` | `WSTRING` | Echtes Funktions-Kommando DOWN (Subscribe). |
| `ID_TASTER_READ_UP` | `WSTRING` | Dritte Kommando-Quelle UP, z. B. Hardware-Taster (Subscribe). |
| `ID_TASTER_READ_DOWN` | `WSTRING` | Dritte Kommando-Quelle DOWN, z. B. Hardware-Taster (Subscribe). |
| `ID_WRITE_DOWN` | `WSTRING` | Echte Funktions-Rückmeldung DOWN (Publish). |
| `QI` | `BOOL` | Qualifier für die physischen Ausgänge - FALSE hält diese Instanz komplett inaktiv (keine Hardware-Bindung), für den Fall, dass derselbe physische Kanal je nach Maschinenvariante anderweitig genutzt wird. Initialwert `TRUE`. |

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `QO` | `BOOL` | Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde (UND aller Subscribe-/Publish-/DigitalOutput-Instanzen). |

### **Adapter**

Keine vorhanden.

## Verwendete Funktionsbausteine (FBs)

- **SUBSCRIBE_TEST_UP / SUBSCRIBE_TEST_DOWN** (`adapter::net::AX_SUBSCRIBE_1`): IO-Test-Kommando je Richtung (`ID_TEST_READ_UP` / `ID_TEST_READ_DOWN`).
- **SUBSCRIBE_CMD_UP / SUBSCRIBE_CMD_DOWN** (`adapter::net::AX_SUBSCRIBE_1`): echtes Funktions-Kommando je Richtung (`ID_READ_UP` / `ID_READ_DOWN`).
- **SUBSCRIBE_TASTER_UP / SUBSCRIBE_TASTER_DOWN** (`adapter::net::AX_SUBSCRIBE_1`): dritte Kommando-Quelle je Richtung (`ID_TASTER_READ_UP` / `ID_TASTER_READ_DOWN`).
- **OR_UP / OR_DOWN** (`adapter::booleanOperators::AX_OR_3`): führen je Richtung Test, Funktion und Taster zusammen.
- **ILOCK** (`logiBUS::signalprocessing::interlock::ILOCK_SWITCH_PROTECT_AX`): Interlock mit Schutzzeit `DT_PROTECT`.
- **E_TimeOut** (`iec61499::events::E_TimeOut`): Zeitgeber für die Schutzzeit, angeschlossen an `ILOCK.timeOut`.
- **SPLIT_UP / SPLIT_DOWN** (`adapter::events::unidirectional::AX_SPLIT_3`): verteilen den interlockten Zustand je Richtung auf drei Ziele.
- **DigitalOutput_UP / DigitalOutput_DOWN** (`logiBUS::io::DQ::logiBUS_QXA`): physische Ausgänge.
- **PUBLISH_TEST_UP / PUBLISH_TEST_DOWN** (`adapter::net::AX_PUBLISH_1`): IO-Test-Rückmeldung (`ID_TEST_WRITE_UP` / `ID_TEST_WRITE_DOWN`).
- **PUBLISH_STATE_UP / PUBLISH_STATE_DOWN** (`adapter::net::AX_PUBLISH_1`): Funktions-Rückmeldung (`ID_WRITE_UP` / `ID_WRITE_DOWN`).
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_12`): verknüpft die `QO` der zwölf Subscribe-/Publish-/DigitalOutput-Instanzen.

## Programmablauf und Verbindungen

1. Initialisierungskette: `INIT` → `SUBSCRIBE_TEST_UP.INIT` → `SUBSCRIBE_CMD_UP` → `SUBSCRIBE_TEST_DOWN` → `SUBSCRIBE_CMD_DOWN` → `SUBSCRIBE_TASTER_UP` → `SUBSCRIBE_TASTER_DOWN` → `PUBLISH_TEST_UP` → `PUBLISH_STATE_UP` → `PUBLISH_TEST_DOWN` → `PUBLISH_STATE_DOWN` → `DigitalOutput_UP` → `DigitalOutput_DOWN` (jeweils `INITO` → nächstes `INIT`); `DigitalOutput_DOWN.INITO` → `INITO` und → `AND_QO.REQ`.
2. Kommandos UP: `SUBSCRIBE_TEST_UP.OUT` → `OR_UP.IN1`, `SUBSCRIBE_CMD_UP.OUT` → `OR_UP.IN2`, `SUBSCRIBE_TASTER_UP.OUT` → `OR_UP.IN3`; `OR_UP.OUT` → `ILOCK.UP_IN`.
3. Kommandos DOWN: `SUBSCRIBE_TEST_DOWN.OUT` → `OR_DOWN.IN1`, `SUBSCRIBE_CMD_DOWN.OUT` → `OR_DOWN.IN2`, `SUBSCRIBE_TASTER_DOWN.OUT` → `OR_DOWN.IN3`; `OR_DOWN.OUT` → `ILOCK.DOWN_IN`.
4. Schutzzeit: `DT_PROTECT` → `ILOCK.DT_PROTECT`; `ILOCK.timeOut` → `E_TimeOut.TimeOutSocket`.
5. Verteilung UP: `ILOCK.UP_OUT` → `SPLIT_UP.IN`; `SPLIT_UP.OUT1` → `DigitalOutput_UP.OUT`, `SPLIT_UP.OUT2` → `PUBLISH_TEST_UP.IN`, `SPLIT_UP.OUT3` → `PUBLISH_STATE_UP.IN`.
6. Verteilung DOWN: `ILOCK.DOWN_OUT` → `SPLIT_DOWN.IN`; `SPLIT_DOWN.OUT1` → `DigitalOutput_DOWN.OUT`, `SPLIT_DOWN.OUT2` → `PUBLISH_TEST_DOWN.IN`, `SPLIT_DOWN.OUT3` → `PUBLISH_STATE_DOWN.IN`.
7. Adressen: `ID_TEST_READ_UP` → `SUBSCRIBE_TEST_UP.ID`, `ID_READ_UP` → `SUBSCRIBE_CMD_UP.ID`, `ID_TASTER_READ_UP` → `SUBSCRIBE_TASTER_UP.ID`, `ID_TEST_READ_DOWN` → `SUBSCRIBE_TEST_DOWN.ID`, `ID_READ_DOWN` → `SUBSCRIBE_CMD_DOWN.ID`, `ID_TASTER_READ_DOWN` → `SUBSCRIBE_TASTER_DOWN.ID`, `ID_TEST_WRITE_UP` → `PUBLISH_TEST_UP.ID`, `ID_WRITE_UP` → `PUBLISH_STATE_UP.ID`, `ID_TEST_WRITE_DOWN` → `PUBLISH_TEST_DOWN.ID`, `ID_WRITE_DOWN` → `PUBLISH_STATE_DOWN.ID`.
8. Ausgänge: `Output_UP` → `DigitalOutput_UP.Output`; `Output_DOWN` → `DigitalOutput_DOWN.Output`.
9. Qualifier: `QI` → `DigitalOutput_UP.QI`, `DigitalOutput_DOWN.QI` und die `QI` aller sechs Subscribe- und vier Publish-Instanzen.
10. Qualitätsauswertung: `SUBSCRIBE_TEST_UP.QO` → `AND_QO.IN1`, `SUBSCRIBE_CMD_UP.QO` → `IN2`, `SUBSCRIBE_TEST_DOWN.QO` → `IN3`, `SUBSCRIBE_CMD_DOWN.QO` → `IN4`, `PUBLISH_TEST_UP.QO` → `IN5`, `PUBLISH_STATE_UP.QO` → `IN6`, `PUBLISH_TEST_DOWN.QO` → `IN7`, `PUBLISH_STATE_DOWN.QO` → `IN8`, `DigitalOutput_UP.QO` → `IN9`, `DigitalOutput_DOWN.QO` → `IN10`, `SUBSCRIBE_TASTER_UP.QO` → `IN11`, `SUBSCRIBE_TASTER_DOWN.QO` → `IN12`; `AND_QO.OUT` → `QO`.

## Technische Besonderheiten

- Gegenüber dem Original sind die Eingänge `ID_TASTER_READ_UP` / `ID_TASTER_READ_DOWN` neu, `OR_UP` / `OR_DOWN` sind jetzt `AX_OR_3` (vorher `AX_OR_2`) und `AND_QO` ist jetzt `AND_BOOL_12`.
- Die Rückmeldungen werden immer vom tatsächlichen, interlockten Zustand abgeleitet.
- Der Interlock verhindert, dass beide Richtungen dauerhaft gleichzeitig aktiv sind; vor einem Richtungswechsel wird die Schutzzeit `DT_PROTECT` eingehalten.

## Anwendungsszenarien

- Doppelwirkender Ausgang (z. B. Ventil mit zwei Spulen), der zusätzlich zu Funktions- und IO-Test-Kommando von einem Hardware-Taster einer anderen Station bedient wird (Erstnutzung: Reihenverstellung KVE16).
- Bedienung eines „digital DW“-Aktors von einem Modul ohne eigenes VT.

## Zusammenfassung

`ILOCK_SWITCH_2_QXA_TASTER_OPC` erweitert [`ILOCK_SWITCH_2_QXA_OPC`](./ILOCK_SWITCH_2_QXA_OPC.md) um einen dritten Kommando-Eingang je Richtung (Taster) und führt Test, Funktion und Taster per `AX_OR_3` vor dem Interlock zusammen.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
