# AR_Timeout_Zero

![AR_Timeout_Zero_network](./AR_Timeout_Zero_network.svg)

* * * * * * * * * *

## Einleitung

`AR_Timeout_Zero` liefert den `REAL`-Wert `IN0` unverändert durch, außer das `AX`-Signal `G` (in der Regel `IA_*.TIMEOUT`, ein echter Kommunikationsausfall) ist `TRUE`: Dann wird `0.0` ausgegeben statt des sonst einfach eingefrorenen letzten Werts. Die Schaltung stammt aus der Fendt-Übung `Uebung_074_AUI`.

![AR_Timeout_Zero](AR_Timeout_Zero.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AR_Timeout_Zero

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **LIT_Zero**: `adapter::types::unidirectional::AR::initval::initval_AR` mit `INIT_VAL=0.0` – liefert die Konstante `0.0`
    - **Selector_AR**: `adapter::iec61131::selection::AR_AX_SEL_AR` – wählt anhand von `G` zwischen `IN0` und `IN1`
- **Funktionsweise**: Der Selektor gibt bei `G=FALSE` den Live-Wert `IN0`, bei `G=TRUE` die Konstante `0.0` aus.

## Schnittstellenstruktur

### **Ereignis-Eingänge / Ereignis-Ausgänge**

Keine – die SubApp hat keine eigenen Ereignis-Ports; Ereignisse laufen über die Adapter.

### **Daten-Eingänge / Daten-Ausgänge**

Keine direkten Daten-Ports.

### **Adapter**

- **IN0** (Socket): `adapter::types::unidirectional::AR` – Live-Wert, z. B. `FIELDBUS_*_SCALED.OUT`
- **G** (Socket): `adapter::types::unidirectional::AX` – `TRUE` = Timeout, liefert `0.0` statt `IN0`
- **OUT** (Plug): `adapter::types::unidirectional::AR` – `IN0`, oder `0.0` bei `G=TRUE`

## Programmablauf und Verbindungen

- `IN0` → `Selector_AR.IN0`
- `LIT_Zero.OUT` → `Selector_AR.IN1`
- `G` → `Selector_AR.G`
- `Selector_AR.OUT` → `OUT`

## Technische Besonderheiten

- Der Baustein ersetzt eine oft wiederkehrende Verdrahtung (`AR_AX_SEL_AR` mit `initval_AR=0`), die zuvor in den TECU-Bausteinen (`TECU_RPTO`, `TECU_FPTO`, `TECU_COGSOG`, `TECU_GBSD`, `TECU_WBSD`, `TECU_VDS`, `TECU_FHS`, `TECU_RHS`, `TECU_MSS`) jeweils einzeln aufgebaut war
- Version 1.1 (2026-09-16): nach `MyLib_AX-1.0.0::sys` verschoben, da generisch und nicht TECU-spezifisch

## Anwendungsszenarien

- Nullsetzen eines Messwerts bei Kommunikationsausfall (Timeout), statt den letzten Wert einzufrieren
- Allgemein: Ersatzwert `0.0` für einen `AR`-Wert, gesteuert durch ein `AX`-Signal

## Zusammenfassung

`AR_Timeout_Zero` kapselt einen `AR`-Selektor mit Konstante `0.0` zu einem wiederverwendbaren Baustein, der bei Timeout den Wert auf Null setzt.

## 🛠️ Zugehörige Übungen

- [Uebung_070d_AUI](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_070d_AUI/)
- [Uebung_079a_AUI](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_079a_AUI/)
