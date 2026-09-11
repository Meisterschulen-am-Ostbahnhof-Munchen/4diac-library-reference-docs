# AID_DM

![AID_DM](./AID_DM.svg)

* * * * * * * * * *

## Einleitung

Der GlobalConstants-Baustein `AID_DM` definiert die Objektattribut-IDs für das ISOBUS-Objekt „Data Mask“ (Datenmaske). Diese Konstanten ermöglichen eine einheitliche und lesbare Referenzierung der Attribute in der Applikationslogik, insbesondere für die Hintergrundfarbe und die zugeordnete Soft-Key-Maske. Sie sind Teil eines umfassenden ISOBUS-Implementierungsrahmens und dienen als zentrale Definitionsquelle für Attributkennungen.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Block handelt, besitzt er keine Ereignis- oder Datenschnittstellen im herkömmlichen Sinne. Die bereitgestellten Werte sind als globale Konstanten im Projekt verfügbar und werden von anderen Bausteinen direkt referenziert.

### **Ereignis-Eingänge**

Nicht vorhanden – es handelt sich um einen reinen Konstantencontainer.

### **Ereignis-Ausgänge**

Nicht vorhanden – es handelt sich um einen reinen Konstantencontainer.

### **Daten-Eingänge**

Nicht vorhanden – es handelt sich um einen reinen Konstantencontainer.

### **Daten-Ausgänge**

Nicht vorhanden – es handelt sich um einen reinen Konstantencontainer.

### **Adapter**

Nicht vorhanden – es handelt sich um einen GlobalConstants-Block.

## Funktionsweise

Der Baustein definiert zwei globale Konstanten:

| Konstante | Datentyp | Initialwert | Beschreibung |
|-----------|----------|-------------|--------------|
| `BACKGROUND_COLOUR` | `USINT` | `USINT#1` | Attribut-ID für den Hintergrundfarbindex der Datenmaske. |
| `SOFT_KEY_MASK` | `USINT` | `USINT#2` | Attribut-ID für die Soft-Key-Maske, die der Datenmaske zugeordnet ist. |

Diese Konstanten können in anderen Bausteinen verwendet werden, um Attributwerte eindeutig zu identifizieren, ohne magische Zahlen im Code zu verwenden. Der Zugriff erfolgt über den qualifizierten Namen, z.B. `AID_DM.BACKGROUND_COLOUR`.

## Technische Besonderheiten

- Die Konstanten sind als `USINT` (Unsigned Short Integer, 8 Bit) definiert und besitzen feste Initialwerte.
- Der Baustein ist als `GlobalConstants` deklariert, wodurch die Werte zur Laufzeit unveränderlich sind.
- Die Attribut-IDs folgen dem ISOBUS-Standard (ISO 11783) und sind spezifisch für das Objekt „Data Mask“.
- Die Werte sind konsistent mit den in der ISOBUS-Spezifikation festgelegten Attributnummern.

## Zustandsübersicht

Nicht zutreffend – der Baustein besitzt keinen internen Zustandsautomaten. Er stellt ausschließlich konstante Daten bereit.

## Anwendungsszenarien

- **ISOBUS-Datenmasken konfigurieren:** Beim Erstellen oder Bearbeiten einer Datenmaske können die Attributwerte über diese Konstanten gesetzt werden, z.B. um die Hintergrundfarbe zu ändern.
- **Soft-Key-Zuordnung:** Die Verknüpfung einer Soft-Key-Maske mit einer Datenmaske kann über die zugehörige Attribut-ID hergestellt werden.
- **Wartung und Lesbarkeit:** Durch die Verwendung benannter Konstanten wird der Quellcode verständlicher und weniger fehleranfällig, da die numerischen Werte zentral definiert sind.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-Umfeld existieren ähnliche Konstantenblöcke für andere Objekttypen, z.B. `AID_A` für Alarmmasken, `AID_WG` für Working Sets oder `AID_OB` für Objekte. Gemeinsam ist allen, dass sie die Attribut-IDs für die jeweilige Objektklasse definieren. `AID_DM` ist speziell auf die Anforderungen von Datenmasken zugeschnitten, unterscheidet sich jedoch nicht grundlegend in der Struktur – lediglich die enthaltenen Konstanten sind objektspezifisch.

## Fazit

Der GlobalConstants-Baustein `AID_DM` ist ein essentieller Bestandteil einer ISOBUS-Anwendung, die mit Datenmasken arbeitet. Er stellt klar definierte, standardkonforme Attribut-IDs bereit und trägt so zur Robustheit und Wartbarkeit der Software bei. Durch die Verwendung dieser Konstanten wird die Implementierung lesbarer und die Einhaltung des ISOBUS-Standards gewährleistet.
