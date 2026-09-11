# FillWindowFS_AR


![FillWindowFS_AR_network](./FillWindowFS_AR_network.svg)

![FillWindowFS_AR](./FillWindowFS_AR.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **FillWindowFS_AR** dient dazu, ein Objekt vom Typ `FillAttributes` (z. B. zur farblichen Hinterlegung in Bedienoberflächen) in Abhängigkeit eines über einen Adapter zugeführten Realwertes einzufärben. Liegt der Wert innerhalb eines definierten Fensters `[rWindowMin, rWindowMax]`, wird das Objekt grün dargestellt; außerhalb dieses Fensters rot. Die eigentliche Farbänderung übernimmt ein interner `Q_FillAttributes`-Baustein, der mit den entsprechenden Farbkonstanten konfiguriert ist.

Der Baustein ist als SubAppTyp realisiert und nutzt die Adaptertechnik, um den zu vergleichenden Wert flexibel über einen unidirektionalen `AR`-Adapter zu erhalten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock besitzt **keine** Ereignis-Eingänge.

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar                                      |
|------|-------|------------------------------------------------|
| CNF  | Event | Bestätigung der angeforderten Färbung (passthrough vom internen Q_FillAttributes) |

### **Daten-Eingänge**

| Name       | Typ   | Initialwert | Kommentar                                       |
|------------|-------|-------------|------------------------------------------------|
| u16ObjId   | UINT  | ID_NULL     | Objekt-ID des zu färbenden FillAttributes-Objekts |
| rWindowMin | REAL  | –           | Untere Grenze des grünen Fensters                |
| rWindowMax | REAL  | –           | Obere Grenze des grünen Fensters                 |

### **Daten-Ausgänge**

| Name      | Typ    | Kommentar                                       |
|-----------|--------|-------------------------------------------------|
| STATUS    | STRING | Statusmeldung des internen Q_FillAttributes      |
| s16result | INT    | Rückgabewert des internen Q_FillAttributes       |

### **Adapter**

| Name | Typ                              | Kommentar                                      |
|------|----------------------------------|------------------------------------------------|
| rPhys| `adapter::types::unidirectional::AR` | Realwert, dessen Position relativ zum Fenster die Farbe bestimmt |

## Funktionsweise

Der über den Adapter `rPhys` ankommende Realwert wird intern mit einem `AR_SPLIT_2`-Baustein auf zwei Zweige verteilt. In den beiden Zweigen wird der Wert jeweils mit einer Fenstergrenze verglichen:

- `GE_Min` (Größer-Gleich) prüft, ob der Wert ≥ `rWindowMin` ist.
- `LE_Max` (Kleiner-Gleich) prüft, ob der Wert ≤ `rWindowMax` ist.

Die beiden Ergebnisse werden mit einer UND-Verknüpfung (`AX_AND_2`) zusammengeführt. Das resultierende Boolesche Signal steuert einen Auswahlbaustein (`AX_SEL`), der zwischen der roten und der grünen Farbkonstante umschaltet. Ist das Signal `TRUE`, wird `COLOR_GREEN` gewählt, andernfalls `COLOR_RED`.

Die ausgewählte Farbe wird zusammen mit der Objekt-ID an den internen `Q_FillAttributes`-Baustein übergeben. Dieser führt die eigentliche Färbung durch und bestätigt die Ausführung über den Ereignisausgang `CNF`. Die Ausgänge `STATUS` und `s16result` übernehmen die vom `Q_FillAttributes` gemeldeten Werte.

Die Fenstergrenzen `rWindowMin` und `rWindowMax` werden über zwei `initval_AR`-Bausteine zu Konstanten fixiert, damit sie während des Vergleichs unverändert bleiben.

## Technische Besonderheiten

- Der Baustein ist als **SubAppTyp** implementiert und nutzt ausschließlich interne Funktionsblöcke – kein eigenständiger Quellcode.
- Die Farben sind als Konstanten `COLOR_GREEN` und `COLOR_RED` vorgegeben und können nicht zur Laufzeit geändert werden.
- Es existieren **keine Ereignis-Eingänge**; die Aktivierung erfolgt implizit durch das Eintreffen neuer Daten am Adapter `rPhys`.
- Die Fensterprüfung ist **inklusiv** (≥ und ≤), d. h. die Grenzen selbst gehören zum grünen Bereich.
- Der interne `Q_FillAttributes`-Baustein wird mit festen Parametern (`u8FillType = USINT#2`, `u16FillPatternId = ID_NULL`) betrieben, die nicht nach außen geführt sind.

## Zustandsübersicht

Der Funktionsblock besitzt keine expliziten internen Zustände. Nach dem Anlegen aller Eingangsdaten (Adapterwert und Fenstergrenzen) wird die Färbung unmittelbar ausgeführt und über `CNF` bestätigt. Es handelt sich um eine rein kombinatorische Funktionalität ohne zeitliche Abhängigkeiten.

## Anwendungsszenarien

- **Visualisierung von Prozesswerten** in HMI-Panels, z. B. Temperatur, Druck oder Füllstand, mit farblicher Kennzeichnung innerhalb eines Sollbereichs.
- **Grenzwertüberwachung** in Maschinensteuerungen, bei der ein zulässiger Betriebsbereich grün und Bereiche außerhalb rot dargestellt werden sollen.
- **Alarm- und Statusanzeigen**, die nur zwischen „OK“ und „Nicht OK“ unterscheiden müssen und keine stufenweisen Zustände erfordern.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu allgemeinen Vergleichs- oder Auswahlbausteinen vereint `FillWindowFS_AR` mehrere Standardfunktionen (Vergleich, UND, Auswahl) in einer spezialisierten Einheit. Gegenüber einem manuell verdrahteten Netzwerk aus einzelnen FBs bietet dieser Baustein eine kompakte, wiederverwendbare Lösung für die beschriebene Färbungsaufgabe. Andere Bausteine, die mit `FillAttributes` arbeiten, geben oft nur eine einzige Farbe vor oder besitzen zusätzliche Ereignis-Eingänge zur gezielten Steuerung; dieser Baustein reagiert rein datengetrieben über das Adaptersignal.

## Fazit

`FillWindowFS_AR` ist ein praxisorientierter Funktionsbaustein, der die einfache farbliche Kennzeichnung eines `FillAttributes`-Objekts in Abhängigkeit eines Fensters realisiert. Durch die Verwendung eines AR-Adapters kann der zu bewertende Wert flexibel aus unterschiedlichen Quellen gespeist werden. Der Baustein ist sofort einsetzbar und reduziert den Verdrahtungsaufwand erheblich, da er die komplette Vergleichs- und Auswahllogik integriert.
