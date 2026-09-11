# AID_OP_PTR

![AID_OP_PTR](./AID_OP_PTR.svg)

* * * * * * * * * *

## Einleitung

Der globale Konstantenbaustein `AID_OP_PTR` definiert einen Konstantenwert für die Objektattribut-ID (Object Pointer Object Attribute ID) im Kontext des ISOBUS-Protokolls (ISO 11783). Er gehört zum Paket `isobus::UT::Q::const::AID` und stellt einen numerischen Identifikator bereit, der in der UT-Anwendung (Universal Terminal) verwendet wird, um auf den „aktuellen Wert“ eines Objektzeigers zu verweisen. Der Baustein ist als `GLOBALCONSTANTS` deklariert, d.h. der enthaltene Wert ist während der gesamten Laufzeit unveränderlich.

## Schnittstellenstruktur

Da es sich um einen globalen Konstantenbaustein handelt, besitzt er **keine** Ein-/Ausgangs-Schnittstellen im Sinne eines Funktionsblocks. Es sind weder Ereignis- noch Datenein-/ausgänge oder Adapter vorhanden.

### **Ereignis-Eingänge**

Nicht vorhanden.

### **Ereignis-Ausgänge**

Nicht vorhanden.

### **Daten-Eingänge**

Nicht vorhanden.

### **Daten-Ausgänge**

Nicht vorhanden.

### **Adapter**

Nicht vorhanden.

## Funktionsweise

Der Baustein `AID_OP_PTR` stellt eine einzige globale Konstante bereit:

- **`VALUE`** (Typ `USINT`, initial `1`)  
  Diese Konstante wird in der ISOBUS-Kommunikation als Attribut-ID für den „aktuellen Wert“ eines Objektzeigers verwendet. Sie ist über den qualifizierten Namen `AID_OP_PTR.VALUE` ansprechbar und kann in Algorithmen oder FB-Instanzen gelesen werden, um den korrekten Attributcode zu referenzieren.

Die Konstante wird zur Compile-Zeit festgelegt und bietet eine zentrale, einheitliche Referenz für alle Module, die mit Objektzeigern arbeiten.

## Technische Besonderheiten

- **Unveränderlichkeit:** Der Wert ist als `CONSTANT` deklariert und kann nicht zur Laufzeit modifiziert werden.
- **Typ:** `USINT` (Unsigned Short Integer, 8 Bit) – Wertebereich 0..255.
- **Initialwert:** `1` gemäß der Kommentierung „1: AID_OP_PTR_VALUE - Current value“.
- **Paketzuordnung:** Der Baustein ist im Paket `isobus::UT::Q::const::AID` definiert, was eine klare Namensraumstruktur für ISOBUS-Attributkonstanten schafft.
- **Copyright/Lizenz:** Die Definition unterliegt der Eclipse Public License 2.0 (SPDX-License-Identifier: EPL-2.0), wie in der XML-Datei vermerkt.

## Zustandsübersicht

Da es sich um eine reine Konstantendefinition handelt, existiert kein internes Zustandsmodell. Der Baustein besitzt keinen Zustand und führt keine Verarbeitungslogik aus.

## Anwendungsszenarien

- In der Implementierung einer ISOBUS-UT-Anwendung, um beim Aufbau von Objektzeigern den Attributwert für „aktueller Wert“ anzugeben.
- Als Ersatz für magische Zahlen im Quellcode, wodurch Wartbarkeit und Lesbarkeit verbessert werden.
- In Verbindung mit anderen Konstantenbausteinen aus demselben Paket (`AID_*`) zur einheitlichen Verwaltung aller Attribut-IDs.

## Vergleich mit ähnlichen Bausteinen

Es gibt im selben Paket vermutlich weitere Konstantenbausteine für andere Objektattribut-IDs (z.B. `AID_OP_PTR` vs. `AID_FCT_CTRL` etc.). Diese unterscheiden sich lediglich im konkreten Wert und in der semantischen Bedeutung. Der Baustein `AID_OP_PTR` ist spezifisch für den Objektzeiger und definiert den Wert `1`. Ähnliche Bausteine folgen demselben Muster, sind aber je nach Attributtyp unterschiedlich benannt und belegt.

## Fazit

`AID_OP_PTR` ist ein einfacher, aber wichtiger globaler Konstantenbaustein für die ISOBUS-Kommunikation. Er stellt einen standardisierten, unveränderlichen Wert für die Objektattribut-ID „aktueller Wert“ bereit und trägt so zur Robustheit und Klarheit von UT-Anwendungen bei. Die strikte Trennung von Konfiguration und Logik wird durch diese Art der Deklaration gefördert.
