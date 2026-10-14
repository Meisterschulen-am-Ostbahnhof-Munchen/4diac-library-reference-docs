# FillWindowFS_AR


![FillWindowFS_AR_network](./FillWindowFS_AR_network.svg)

![FillWindowFS_AR](./FillWindowFS_AR.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **FillWindowFS_AR** dient dazu, ein Objekt vom Typ `FillAttributes` (z. B. zur farblichen Hinterlegung in Bedienoberflächen) in Abhängigkeit eines über einen Adapter zugeführten Realwertes einzufärben. Liegt der Wert innerhalb eines definierten Fensters `[rWindowMin, rWindowMax]`, wird das Objekt grün dargestellt; außerhalb dieses Fensters rot. Die Fensterprüfung übernimmt ein interner `AR_DEADBAND_COMPARE`-Baustein, die eigentliche Farbänderung ein interner `Q_FillAttributes`-Baustein, dem die Farbkonstanten über eine `AX_SEL`-Auswahl zugeführt werden.

Der Baustein ist als SubAppTyp realisiert und nutzt die Adaptertechnik, um den zu vergleichenden Wert flexibel über einen unidirektionalen `AR`-Adapter zu erhalten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ   | Kommentar |
|------|-------|-----------|
| INIT | EInit | Initialisierungsanforderung – wird an den internen `Q_FillAttributes` (`Inner.INIT`) durchgereicht |

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar |
|------|-------|-----------|
| INITO | EInit | Initialisierungsbestätigung (von `Inner.INITO`) |
| CNF  | Event | Bestätigung der angeforderten Färbung (passthrough vom internen Q_FillAttributes) |

### **Daten-Eingänge**

| Name       | Typ   | Initialwert | Kommentar                                       |
|------------|-------|-------------|------------------------------------------------|
| QI         | BOOL  | TRUE        | Quality Input – TRUE, um den Dienst freizugeben (an `Inner.QI`) |
| u16ObjId   | UINT  | ID_NULL     | Objekt-ID des zu färbenden FillAttributes-Objekts |
| rWindowMin | REAL  | –           | Untere Grenze des grünen Fensters                |
| rWindowMax | REAL  | –           | Obere Grenze des grünen Fensters                 |

### **Daten-Ausgänge**

| Name      | Typ    | Kommentar                                       |
|-----------|--------|-------------------------------------------------|
| QO        | BOOL   | Quality Output – TRUE, wenn der Dienst erfolgreich ausgeführt wurde (von `Inner.QO`) |
| STATUS    | STRING | Statusmeldung des internen Q_FillAttributes      |
| s16result | INT    | Rückgabewert des internen Q_FillAttributes       |

### **Adapter**

| Name | Typ                              | Kommentar                                      |
|------|----------------------------------|------------------------------------------------|
| rPhys| `adapter::types::unidirectional::AR` | Realwert, dessen Position relativ zum Fenster die Farbe bestimmt |

## Funktionsweise

Der über den Adapter `rPhys` ankommende Realwert wird intern einem `AR_DEADBAND_COMPARE`-Baustein (`CENTER_CMP`) als Messwert `Y` zugeführt. Die Fenstergrenzen `rWindowMin` und `rWindowMax` werden über zwei `initval_AR`-Bausteine (`WindowMinConst`, `WindowMaxConst`) in AR-Adapter-Werte gewandelt und an `WINDOW_MIN` bzw. `WINDOW_MAX` von `CENTER_CMP` gelegt.

`AR_DEADBAND_COMPARE` meldet über den AX-Plug `CENTERED`, ob `WINDOW_MIN ≤ Y ≤ WINDOW_MAX` gilt (die Plugs `ABOVE`/`BELOW` werden hier nicht verwendet). `CENTERED` steuert den Auswahlbaustein `ColorSel` (`AX_SEL`), der zwischen der roten und der grünen Farbkonstante umschaltet: `TRUE` → `COLOR_GREEN` (`IN1`), `FALSE` → `COLOR_RED` (`IN0`).

Die ausgewählte Farbe wird zusammen mit der Objekt-ID an den internen `Q_FillAttributes`-Baustein (`Inner`) übergeben: `ColorSel.CNF` → `Inner.REQ`, `ColorSel.OUT` → `Inner.u8FillColour`, `u16ObjId` → `Inner.u16ObjId`. Dieser führt die eigentliche Färbung durch und bestätigt die Ausführung über den Ereignisausgang `CNF`. Die Ausgänge `STATUS` und `s16result` übernehmen die vom `Q_FillAttributes` gemeldeten Werte.

Initialisierung und Quality-Signale werden direkt durchgereicht: `INIT` → `Inner.INIT`, `Inner.INITO` → `INITO`, `QI` → `Inner.QI`, `Inner.QO` → `QO`.

## Technische Besonderheiten

- Der Baustein ist als **SubAppTyp** implementiert und nutzt ausschließlich interne Funktionsblöcke – kein eigenständiger Quellcode.
- Die Farben sind als Konstanten `COLOR_GREEN` und `COLOR_RED` vorgegeben und können nicht zur Laufzeit geändert werden.
- Außer `INIT` gibt es **keine Ereignis-Eingänge**; die Färbung wird durch das Eintreffen neuer Daten am Adapter `rPhys` (bzw. an den Fenstergrenzen) ausgelöst.
- Die Fensterprüfung ist **inklusiv** (≥ und ≤), d. h. die Grenzen selbst gehören zum grünen Bereich. Sie übernimmt `AR_DEADBAND_COMPARE` (ab Version 2.0 des Bausteins, vorher eigene Split-/GE-/LE-/UND-Verdrahtung).
- Der interne `Q_FillAttributes`-Baustein wird mit festen Parametern (`u8FillType = USINT#2`, `u16FillPatternId = ID_NULL`) betrieben, die nicht nach außen geführt sind.
- Quality-Muster: `QI` (Vorgabe `TRUE`) gibt den Dienst frei, `QO` meldet den Erfolg des internen `Q_FillAttributes`.

## Zustandsübersicht

Der Funktionsblock besitzt keine expliziten internen Zustände (der Vergleich in `AR_DEADBAND_COMPARE` meldet nur Änderungen seiner Ausgänge). Nach dem Anlegen aller Eingangsdaten (Adapterwert und Fenstergrenzen) wird die Färbung ausgeführt und über `CNF` bestätigt; `INIT`/`INITO` werden nur durchgereicht.

## Anwendungsszenarien

- **Visualisierung von Prozesswerten** in HMI-Panels, z. B. Temperatur, Druck oder Füllstand, mit farblicher Kennzeichnung innerhalb eines Sollbereichs.
- **Grenzwertüberwachung** in Maschinensteuerungen, bei der ein zulässiger Betriebsbereich grün und Bereiche außerhalb rot dargestellt werden sollen.
- **Alarm- und Statusanzeigen**, die nur zwischen „OK“ und „Nicht OK“ unterscheiden müssen und keine stufenweisen Zustände erfordern.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu allgemeinen Vergleichs- oder Auswahlbausteinen vereint `FillWindowFS_AR` mehrere Standardfunktionen (Fenstervergleich, Auswahl) in einer spezialisierten Einheit. Gegenüber einem manuell verdrahteten Netzwerk aus einzelnen FBs bietet dieser Baustein eine kompakte, wiederverwendbare Lösung für die beschriebene Färbungsaufgabe. Andere Bausteine, die mit `FillAttributes` arbeiten, geben oft nur eine einzige Farbe vor oder besitzen zusätzliche Ereignis-Eingänge zur gezielten Steuerung; dieser Baustein reagiert rein datengetrieben über das Adaptersignal.

## Fazit

`FillWindowFS_AR` ist ein praxisorientierter Funktionsbaustein, der die einfache farbliche Kennzeichnung eines `FillAttributes`-Objekts in Abhängigkeit eines Fensters realisiert. Durch die Verwendung eines AR-Adapters kann der zu bewertende Wert flexibel aus unterschiedlichen Quellen gespeist werden. Der Baustein ist sofort einsetzbar und reduziert den Verdrahtungsaufwand erheblich, da er die komplette Vergleichs- und Auswahllogik integriert.

