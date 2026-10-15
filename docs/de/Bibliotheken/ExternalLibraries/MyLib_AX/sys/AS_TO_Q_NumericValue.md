# AS_TO_Q_NumericValue


![AS_TO_Q_NumericValue_network](./AS_TO_Q_NumericValue_network.svg)

![AS_TO_Q_NumericValue](./AS_TO_Q_NumericValue.svg)

* * * * * * * * * *

## Einleitung

Der Subapp-Baustein **AS_TO_Q_NumericValue** dient zur Anzeige eines AS-Adapter-Werts (beispielsweise einer Schrittkettennummer) über einen AUDI-Konverter auf einem Visualisierungs-Zahlenfeld (VT-Zahlenfeld) vom Typ `Q_NumericValue_AUDI`. Er ist generisch aufgebaut und die Ziel-Object-ID ist über den Eingang `u16ObjId` parametrierbar. Dieser Baustein ist die Schwester-Variante zu `MyLib::sys::SINT_TO_Q_NumericValue`, bei dem anstelle des AS-Adapters eine direkte SINT-Verdrahtung verwendet wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung – wird an `Q_NumericValue_AUDI.INIT` durchgereicht.

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung – kommt von `Q_NumericValue_AUDI.INITO`.

### **Daten-Eingänge**

| Name      | Datentyp | Initialwert | Beschreibung |
|-----------|----------|-------------|--------------|
| `QI`       | `BOOL`   | `TRUE`      | Quality Input – TRUE, um den Dienst freizugeben. |
| `u16ObjId` | `UINT`   | `ID_NULL`   | Object-ID für das anzuzeigende Zahlenfeld. Über diese ID wird das Zielgerät bzw. die Zielvariable in der Visualisierung adressiert. |

### **Daten-Ausgänge**

| Name | Datentyp | Beschreibung |
|------|----------|--------------|
| `QO` | `BOOL`   | Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde (von `Q_NumericValue_AUDI.QO`). |

### **Adapter**

| Name        | Typ                                        | Richtung    | Beschreibung |
|-------------|--------------------------------------------|-------------|--------------|
| `STATE_NR`  | `adapter::types::unidirectional::AS`       | Eingang (Socket) | Adapter für den AS-Wert (z.B. aktuelle Schrittkettennummer). |

## Funktionsweise

Der Subapp-Baustein nimmt über den Adapter `STATE_NR` einen AS-Wert entgegen. Dieser wird durch den internen FB `AS_TO_AUDI` (Typ `adapter::conversion::unidirectional::AS_TO_AUDI`) in ein AUDI-Signal umgewandelt. Das konvertierte Signal (`AUDI_OUT`) wird an den FB `Q_NumericValue_AUDI` (Typ `isobus::UT::Q::Q_NumericValue_AUDI`) übergeben, welcher den Wert in ein VT-Zahlenfeld schreibt. Die Ziel-Object-ID wird dabei über den Eingang `u16ObjId` gesetzt und direkt mit dem FB `Q_NumericValue_AUDI` verbunden.

Zusätzlich sind die Initialisierung und die Quality-Signale durchgereicht: `INIT` → `Q_NumericValue_AUDI.INIT`, `Q_NumericValue_AUDI.INITO` → `INITO`, `QI` → `Q_NumericValue_AUDI.QI` und `Q_NumericValue_AUDI.QO` → `QO`. Die eigentliche Wertübertragung bleibt datengetrieben über Daten- und Adapterverbindungen.

## Technische Besonderheiten

- **Generische Parametrierung**: Durch den Eingang `u16ObjId` kann der Baustein für beliebige VT-Zahlenfelder verwendet werden, ohne dass eine feste Object-ID hartkodiert ist.
- **Adapter-Konvertierung**: Die Kombination aus `AS_TO_AUDI` und `Q_NumericValue_AUDI` ermöglicht die Anbindung von AS-Adapter-Daten an das AUDI-Protokoll der Visualisierung.
- **Wiederverwendbarkeit**: Der Baustein wurde aus der Übung `Uebung_039_sub_NumbAnzeig_AS` ausgelagert, um in mehreren Projekten unabhängig einsetzbar zu sein.
- **Datengetriebene Übertragung**: Außer dem Initialisierungspfad (`INIT`/`INITO`) gibt es keine Ereignisverdrahtung; die Werte werden über Daten- und Adapterverbindungen übertragen, was eine einfache und robuste Integration erlaubt.
- **Quality-Muster**: `QI` (Vorgabe `TRUE`) gibt den Dienst frei, `QO` meldet den Erfolg des inneren `Q_NumericValue_AUDI`.
- **Import**: Enthält einen Import für `isobus::UT::Q::const::IDs::ID_NULL` zur Initialisierung der Object-ID.

## Zustandsübersicht

Da der Baustein keine Zustandsautomaten besitzt (`INIT`/`INITO` werden nur durchgereicht), gibt es keine expliziten Zustände. Die Funktionalität ist rein kombinatorisch und abhängig von den aktuellen Eingangswerten. Änderungen am Adapterwert oder an `u16ObjId` wirken sich unmittelbar auf die Ausgabe im Zahlenfeld aus.

## Anwendungsszenarien

- **Visualisierung von Schrittkettennummern**: Anzeige der aktuellen Schrittnummer einer SPS-Steuerung über einen AS-Adapter auf einem HMI-Bildschirm.
- **Generische Werteanzeige**: Der Baustein kann verwendet werden, um beliebige numerische Werte (z.B. Messwerte, Zählerstände) über AS-Schnittstellen auf VT-Zahlenfelder zu projizieren, sofern eine AUDI-Konvertierung verfügbar ist.
- **Modulare HMI-Anbindung**: Durch die Parametrierbarkeit der Object-ID eignet sich der Baustein für den Einsatz in wiederverwendbaren Funktionsbaustein-Bibliotheken für Maschinen- oder Anlagenvisualisierungen.

## Vergleich mit ähnlichen Bausteinen

| Merkmal                  | `AS_TO_Q_NumericValue`                     | `SINT_TO_Q_NumericValue` (Schwester)      |
|--------------------------|--------------------------------------------|-------------------------------------------|
| Eingangsquelle           | AS-Adapter                                  | Direkter SINT-Wert                        |
| Konvertierung            | `AS_TO_AUDI` → `Q_NumericValue_AUDI`       | Keine (SINT wird direkt an `Q_NumericValue_AUDI` übergeben) |
| Parametrierung `u16ObjId` | Ja                                         | Ja                                        |
| Verwendung               | AS-basierte Systeme                        | Direkte SINT-Verdrahtung                  |
| Komplexität              | Höher (Adapterlogik)                        | Geringer                                   |

Beide Bausteine verfolgen das gleiche Ziel – die Anzeige eines Werts auf einem VT-Zahlenfeld – unterscheiden sich aber in der Art der Wertquelle. `AS_TO_Q_NumericValue` ist für Systeme gedacht, die über AS-Adapter kommunizieren, während die Schwester-Variante für einfache SINT-Signale optimiert ist.

## Fazit

Der Subapp-Baustein `AS_TO_Q_NumericValue` bietet eine flexible und wiederverwendbare Lösung zur Anzeige von AS-Adapter-Werten auf VT-Zahlenfeldern. Durch die generische Object-ID-Ansteuerung und die klare Trennung von Adapterkonvertierung und Visualisierung eignet er sich hervorragend für modulare Automatisierungsprojekte. Die Einbettung in die Bibliothek `MyLib::sys` gewährleistet eine konsistente Integration und Reduzierung von Redundanz in zukünftigen Anwendungen.
