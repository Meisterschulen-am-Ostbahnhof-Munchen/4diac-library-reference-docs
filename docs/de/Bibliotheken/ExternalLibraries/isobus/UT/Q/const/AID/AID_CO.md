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

Der Baustein stellt drei globale Konstanten bereit, die die Attribut-IDs für Container-Objekte im ISOBUS-System repräsentieren (ISO 11783-6 Tabelle B.8). Diese Konstanten werden bei der Erstellung des Objekt-Pools (Object Pool Construction) verwendet, um die initialen Attribute eines Containers zu definieren.

Zur Laufzeit sind alle drei Attribute (`WIDTH`, `HEIGHT` und `HIDDEN`) für den Befehl *Change Attribute* (F.38) **schreibgeschützt (read-only)**. Ein Versuch, die Abmessungen eines Containers zur Laufzeit über *Change Attribute* zu verändern, ist nach ISO 11783-6 nicht zulässig. Die Verwendung von `WIDTH` und `HEIGHT` zur Laufzeit beschränkt sich auf das Abfragen der Containerdimensionen mittels *Get Attribute Value* (F.58).

Die definierten Konstanten:

- `WIDTH` mit dem Wert `[1]` – **Read-only** für *Change Attribute* (ISO 11783-6 Tabelle B.8). Maximale Breite des Containerbereichs in Pixeln. Kann bei der Objektpool-Erstellung angegeben und zur Laufzeit via *Get Attribute Value* (F.58) abgefragt werden.
- `HEIGHT` mit dem Wert `[2]` – **Read-only** für *Change Attribute* (ISO 11783-6 Tabelle B.8). Maximale Höhe des Containerbereichs in Pixeln. Kann bei der Objektpool-Erstellung angegeben und zur Laufzeit via *Get Attribute Value* (F.58) abgefragt werden.
- `HIDDEN` mit dem Wert `[3]` – **Read-only** für *Change Attribute* (ISO 11783-6 Tabelle B.8). Abfragbar via *Get Attribute Value* (F.58); dynamische Sichtbarkeitsänderungen erfolgen ausschließlich über den speziellen Befehl *Hide/Show Object* (F.2) (0 = sichtbar, 1 = verborgen).

## Technische Besonderheiten

- Der Baustein ist als GlobalConstants-Element definiert, das in der gesamten 4diac‑Projektstruktur verwendet werden kann.
- Die Konstanten sind vom Typ `USINT` (Unsigned Short Integer) und mit Initialwerten versehen, die den ISOBUS‑Spezifikationen entsprechen.
- Der Baustein enthält keinerlei ausführbare Logik – er ist rein deklarativ.
- Die Kommentare zu den Konstanten beschreiben die zugrunde liegenden Attribut-Bedeutungen und erleichtern die Wiederverwendung.

## Zustandsübersicht

Da es sich um einen Konstantencontainer handelt, existiert kein Zustandsmodell. Der Baustein besitzt keinen internen Zustand und führt keine Zustandsübergänge aus. Die Werte der Konstanten sind statisch und unveränderlich.

## Anwendungsszenarien

- **ISOBUS-Objektpool-Erstellung**: Verwendung von `AID_CO.WIDTH`, `AID_CO.HEIGHT` und `AID_CO.HIDDEN` bei der Definition von Container-Objektattributen im Objekt-Pool für ein Virtuelles Terminal.
- **Laufzeit-Attributabfrage**: Verwendung von `AID_CO.WIDTH`, `AID_CO.HEIGHT` und `AID_CO.HIDDEN` beim Abfragen von Container-Eigenschaften über den Befehl *Get Attribute Value* (F.58). Schreibzugriffe via *Change Attribute* (F.38) sind für diese Attribute nicht unterstützt.
- **Sichtbarkeitssteuerung**: Dynamisches Ein- und Ausblenden von Containern zur Laufzeit erfolgt über den Befehl *Hide/Show Object* (F.2) statt über *Change Attribute*.

## Vergleich mit ähnlichen Bausteinen

Es gibt weitere GlobalConstants-Bausteine, die ähnliche Attribut-IDs für andere ISOBUS-Objekttypen definieren, z. B. `AID_OBJ` für Objekt-Attribute oder `AID_POOL`. Im Vergleich zu diesen enthält `AID_CO` spezifisch die Kennungen für Container-Objekte. Andere Bausteine besitzen möglicherweise zusätzliche Konstanten (z. B. für Position, Farbe oder Text), während `AID_CO` nur die drei Grundattribute des Containers abdeckt.

## Fazit

Der Baustein `AID_CO` stellt eine einfache, aber wichtige Grundlage für die ISOBUS-basierte Entwicklung in 4diac dar. Durch die klare Definition der Attribut-IDs wird die Lesbarkeit und Wartbarkeit des Projekts erhöht und die Gefahr von Fehlern durch falsche Zahlenwerte reduziert. Obwohl er keine Funktionalität im klassischen Sinne besitzt, ist er ein unverzichtbares Hilfsmittel für die konsistente Kommunikation mit landwirtschaftlichen Terminals.
