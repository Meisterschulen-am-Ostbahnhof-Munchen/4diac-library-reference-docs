# AID_CO

![AID_CO](./AID_CO.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_CO` ist eine Sammlung globaler Konstanten, die in der ISOBUS-Kommunikation (ISO 11783) für Container-Objekte verwendet werden. Er definiert numerische Kennungen (Attribute Identifiers) für Eigenschaften eines Containers innerhalb einer landwirtschaftlichen Maschine. Diese Konstanten dienen als symbolische Referenzen, um in anderen Funktionsbausteinen die entsprechenden Attribut-IDs nicht als magische Zahlen verwenden zu müssen. Der Baustein ist als reiner Konstantencontainer ohne eigene Logik oder Schnittstellen ausgelegt.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Es sind keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**

Es sind keine Ereignis-Ausgänge vorhanden.

### **Daten-Eingänge**

Es sind keine Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

Es sind keine Daten-Ausgänge vorhanden.

### **Adapter**

Es sind keine Adapter vorhanden.

## Funktionsweise

Der Baustein stellt drei globale Konstanten bereit, die die Attribut-IDs für Container-Objekte im ISOBUS-System repräsentieren. Diese Konstanten sind direkt in allen anderen Anwendungen des Projekts zugreifbar und können beispielsweise in Daten-Kommunikationsbausteinen oder Verarbeitungslogiken verwendet werden, um eine einheitliche und verständliche Referenz auf die jeweilige Eigenschaft eines Containers zu ermöglichen.

Die definierten Konstanten:

- `WIDTH` mit dem Wert `1` – steht für die maximale Breite des Containerbereichs in Pixeln.
- `HEIGHT` mit dem Wert `2` – steht für die maximale Höhe des Containerbereichs in Pixeln.
- `HIDDEN` mit dem Wert `3` – kennzeichnet, ob der Container und seine untergeordneten Objekte verborgen sind (0 = sichtbar, 1 = verborgen).

## Technische Besonderheiten

- Der Baustein ist als GlobalConstants-Element definiert, das in der gesamten 4diac‑Projektstruktur verwendet werden kann.
- Die Konstanten sind vom Typ `USINT` (Unsigned Short Integer) und mit Initialwerten versehen, die den ISOBUS‑Spezifikationen entsprechen.
- Der Baustein enthält keinerlei ausführbare Logik – er ist rein deklarativ.
- Die Kommentare zu den Konstanten beschreiben die zugrunde liegenden Attribut-Bedeutungen und erleichtern die Wiederverwendung.

## Zustandsübersicht

Da es sich um einen Konstantencontainer handelt, existiert kein Zustandsmodell. Der Baustein besitzt keinen internen Zustand und führt keine Zustandsübergänge aus. Die Werte der Konstanten sind statisch und unveränderlich.

## Anwendungsszenarien

- **ISOBUS-Datenkommunikation**: Wenn ein ISOBUS-Netzwerk (z. B. ein Terminal) Daten über Container-Objekte austauscht, können diese Konstanten verwendet werden, um die entsprechenden Attribut-IDs in Nachrichten zu referenzieren.
- **Bildschirmdarstellung**: Bei der Darstellung eines Containers auf einem virtuellen Terminal können die Konstanten `WIDTH` und `HEIGHT` zur Berechnung der Anzeigegröße herangezogen werden.
- **Sichtbarkeitssteuerung**: Die Konstante `HIDDEN` kann verwendet werden, um über einen Logikbaustein die Sichtbarkeit eines Containers dynamisch zu steuern.

## Vergleich mit ähnlichen Bausteinen

Es gibt weitere GlobalConstants-Bausteine, die ähnliche Attribut-IDs für andere ISOBUS-Objekttypen definieren, z. B. `AID_OBJ` für Objekt-Attribute oder `AID_POOL`. Im Vergleich zu diesen enthält `AID_CO` spezifisch die Kennungen für Container-Objekte. Andere Bausteine besitzen möglicherweise zusätzliche Konstanten (z. B. für Position, Farbe oder Text), während `AID_CO` nur die drei Grundattribute des Containers abdeckt.

## Fazit

Der Baustein `AID_CO` stellt eine einfache, aber wichtige Grundlage für die ISOBUS-basierte Entwicklung in 4diac dar. Durch die klare Definition der Attribut-IDs wird die Lesbarkeit und Wartbarkeit des Projekts erhöht und die Gefahr von Fehlern durch falsche Zahlenwerte reduziert. Obwohl er keine Funktionalität im klassischen Sinne besitzt, ist er ein unverzichtbares Hilfsmittel für die konsistente Kommunikation mit landwirtschaftlichen Terminals.
