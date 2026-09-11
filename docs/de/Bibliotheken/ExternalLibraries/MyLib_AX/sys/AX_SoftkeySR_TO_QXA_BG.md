# AX_SoftkeySR_TO_QXA_BG


![AX_SoftkeySR_TO_QXA_BG_network](./AX_SoftkeySR_TO_QXA_BG_network.svg)

![AX_SoftkeySR_TO_QXA_BG](./AX_SoftkeySR_TO_QXA_BG.svg)

* * * * * * * * * *

## Einleitung

Die Subapplikation **AX_SoftkeySR_TO_QXA_BG** ermöglicht die Steuerung eines digitalen Ausgangs (QXA) über drei Softkeys. Sie kombiniert die Funktionen *SET*, *RESET* und *TOGGLE* unter Verwendung eines SR-Flip-Flops und eines Signal-Splitters. Zusätzlich wird ein Hintergrund-Submodul angesteuert, das bei jedem Toggle-Schritt den Zustand des Hintergrunds wechselt. Die Bausteine sind generisch konfigurierbar und für eine Wiederverwendung in verschiedenen Applikationen ausgelegt.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

| Name          | Typ                                  | Initialwert | Kommentar                                                        |
|---------------|--------------------------------------|-------------|------------------------------------------------------------------|
| `u16ObjId_SET`   | `UINT`                               | `ID_NULL`   | Objekt-ID des Softkeys für die SET-Funktion.                     |
| `u16ObjId_RESET` | `UINT`                               | `ID_NULL`   | Objekt-ID des Softkeys für die RESET-Funktion.                   |
| `u16ObjId_TOGGLE`| `UINT`                               | `ID_NULL`   | Objekt-ID des Softkeys für die TOGGLE-Funktion (auch für Hintergrund). |
| `Output`         | `logiBUS::io::DQ::logiBUS_DO_S`     | `Invalid`   | Ausgangskanal (z. B. `Output_Q1` … `Output_Q8`) für die digitale Ausgabe. |

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine (interne Adapter werden verwendet, aber nicht nach außen geführt).

## Funktionsweise

Die Subapplikation besitzt drei interne Softkey-Eingangsbausteine (`SoftKey_SET`, `SoftKey_RESET`, `SoftKey_TOGGLE`), die jeweils einen `Softkey_IE`-Funktionsbaustein repräsentieren. Diese Bausteine werden über die externen Objekt-IDs konfiguriert. Die Ereignisse `IND` der Softkeys werden auf ein SR-Flip-Flop (`AX_T_FF_SR`) geleitet:

- **SET** → setzt den Ausgang `Q` auf TRUE,
- **RESET** → setzt den Ausgang `Q` auf FALSE,
- **TOGGLE** → invertiert den aktuellen Zustand von `Q`.

Der Ausgang `Q` wird über den Adapter `AX_SPLIT_2` auf zwei Pfade aufgeteilt:

1. **Digitaler Ausgang** (`logiBUS_QXA`): Der Wert wird an den über `Output` spezifizierten Kanal weitergegeben.
2. **Hintergrund-Submodul** (`GreenWhiteBackground1_AX`): Dieses erhält zusätzlich die Objekt-ID des TOGGLE-Softkeys, um bei jedem Toggle den Hintergrundzustand (z. B. grün/weiß) zu wechseln.

Die Verbindungen zwischen den Bausteinen sind in der SubApp fest verdrahtet, sodass die gesamte Logik durch die externen Parameter konfiguriert werden kann.

## Technische Besonderheiten

- **Generische Konfiguration:** Die Objekt-IDs und der Ausgangskanal sind als generische Eingänge definiert. Standardmäßig sind sie auf `ID_NULL` bzw. `Invalid` gesetzt, wodurch die SubApp ohne Anpassung keine aktive Funktion besitzt. Erst durch Zuweisung gültiger Werte wird die Steuerung aktiv.
- **Wiederverwendbarkeit:** Die SubApp ist aus einem Übungsprojekt ausgelagert und kann in verschiedenen Anwendungen eingesetzt werden, ohne Änderungen an der internen Logik vorzunehmen.
- **Verwendete Typen:** Die Eingänge nutzen `UINT` und den spezifischen Typ `logiBUS_DO_S` aus der logiBUS-Bibliothek. Intern werden Adapter und Funktionsbausteine aus den Bibliotheken `isobus` und `adapter` verwendet.
- **Signalaufteilung:** Der Splitter `AX_SPLIT_2` ermöglicht die parallele Nutzung eines einzigen Ausgangssignals für zwei Ziele.

## Zustandsübersicht

Die SubApp besitzt einen internen binären Zustand, der durch das SR-Flip-Flop `AX_T_FF_SR` gehalten wird. Die möglichen Zustände sind:

- **Zustand 0 (Reset):** Ausgang `Q` = FALSE.  
  *Übergänge:*  
  - SET-Event → Zustand 1  
  - TOGGLE-Event → Zustand 1  
  - RESET-Event bleibt in Zustand 0
- **Zustand 1 (Set):** Ausgang `Q` = TRUE.  
  *Übergänge:*  
  - RESET-Event → Zustand 0  
  - TOGGLE-Event → Zustand 0  
  - SET-Event bleibt in Zustand 1

Der Ausgang `Q` wird in beide Richtungen über den Splitter an die nachgelagerten Bausteine (Ausgang und Hintergrund) verteilt.

## Anwendungsszenarien

- **Maschinensteuerung:** Ein einzelner Ausgang (z. B. Licht, Ventil) kann über drei verschiedene Softkeys ein- (SET), aus- (RESET) und umgeschaltet (TOGGLE) werden.
- **Visualisierung:** Die TOGGLE-Funktion kann gleichzeitig einen Hintergrundfarbwechsel (z. B. grün/weiß) an einem Bedienpanel auslösen, um den aktuellen Zustand visuell darzustellen.
- **Flexible Zuordnung:** Durch die parametrierbaren Objekt-IDs können die Softkeys je nach Station oder Bedienkontext dynamisch zugewiesen werden.

## Vergleich mit ähnlichen Bausteinen

- **Einfacher SR-Flip-Flop:** Im Gegensatz zu einem nackten SR-Flip-Flop integriert diese SubApp bereits die Softkey-Anbindung und die Ausgangsansteuerung, wodurch weniger externe Verdrahtung nötig ist.
- **Einzel-Softkey-Schalter:** Bausteine, die nur einen einzelnen Softkey verwenden, besitzen keine SET/RESET/Toggle-Kombination. Diese SubApp bietet eine vollständige Drei-Tasten-Bedienung.
- **Mit Hintergrundsteuerung:** Viele ähnliche Bausteine steuern nur einen Ausgang, ohne die zusätzliche Hintergrundlogik. Die Einbindung von `GreenWhiteBackground1_AX` erweitert den Funktionsumfang.

## Fazit

Die SubApplikation **AX_SoftkeySR_TO_QXA_BG** stellt eine kompakte und wiederverwendbare Lösung zur Steuerung eines digitalen Ausgangs über drei Softkeys dar. Sie vereint SR-Flip-Flop-Logik, Ausgangstreiber und eine optionale Hintergrundumschaltung in einem einzigen Modul. Durch die generischen Parameter ist sie flexibel an verschiedene Objekt-IDs und Ausgangskanäle anpassbar und eignet sich daher für vielfältige Anwendungen in der Automatisierungstechnik und Visualisierung.
