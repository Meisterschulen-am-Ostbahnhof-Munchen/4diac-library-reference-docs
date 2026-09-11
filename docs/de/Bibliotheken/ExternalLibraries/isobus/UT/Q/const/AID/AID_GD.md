# AID_GD

![AID_GD](./AID_GD.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_GD` (Attribute ID für Grafikdaten) ist ein globaler Konstantendefinitions-Baustein gemäß IEC 61499. Er stellt konstante Werte bereit, die als Attribut-IDs für die Kommunikation mit ISO-bus-fähigen Geräten dienen. Insbesondere wird hier die Konstante `FORMAT` definiert, die das Grafikformat für PNG-basierte Bilddaten festlegt. Solche Konstanten werden in der Regel in übergeordneten Systemen verwendet, um einheitliche und eindeutige Kennungen für den Austausch von Grafikinformationen zu gewährleisten.

Da es sich um einen `GlobalConstants`-Baustein handelt, werden keine ausführbaren Funktionen oder Zustandsautomaten bereitgestellt. Stattdessen dient er als zentrale Definitionsquelle für wiederverwendbare Konstanten innerhalb einer Applikation.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Es sind keine Ereignis-Eingänge vorhanden. Der Baustein besitzt keine ereignisgesteuerten Schnittstellen.

### **Ereignis-Ausgänge**

Es sind keine Ereignis-Ausgänge vorhanden.

### **Daten-Eingänge**

Es sind keine Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

Es sind keine Daten-Ausgänge vorhanden. Die definierten Konstanten werden nicht über Ausgänge bereitgestellt, sondern stehen global im Kontext des Bausteins zur Verfügung.

### **Adapter**

Es sind keine Adapter vorhanden.

### Globale Konstanten (Definitionen)

| Name | Typ | Initialwert | Kommentar |
|------|-----|-------------|-----------|
| `FORMAT` | `USINT` | `USINT#1` | 1: AID_GD_FORMAT – Grafiktyp: 0 = PNG, beschränkt auf 32-Bit-RGBA Maximum. |

## Funktionsweise

Der Baustein `AID_GD` definiert eine globale Konstante `FORMAT` mit dem Wert 1. Dieser Wert entspricht dem Attribut-Identifier `AID_GD_FORMAT`, der in der ISOBUS-Spezifikation (ISO 11783) für Grafikdatenobjekte verwendet wird. Der Wert 1 kennzeichnet, dass das Grafikformat PNG (Portable Network Graphics) ist, wobei die Farbtiefe auf maximal 32 Bit RGBA begrenzt ist.

Da es sich um einen GlobalConstants-Baustein handelt, werden die Konstanten beim Laden der Applikation in den Speicher übernommen und können von anderen Bausteinen referenziert werden. Sie sind während der gesamten Laufzeit unveränderlich (constant).

## Technische Besonderheiten

- Der Baustein ist als `GlobalConstants` definiert und verwendet das IEC-61499-Format.
- Die Konstante `FORMAT` ist vom Typ `USINT` (Unsigned Short Integer) und wird mit dem Wert 1 initialisiert.
- Die Bedeutung des Wertes ist in der ISOBUS-Norm festgelegt: 0 = PNG, eingeschränkt auf 32-Bit-RGBA-Maximum.
- Durch die Verwendung als globale Konstante wird vermieden, dass der Wert mehrfach an verschiedenen Stellen hartcodiert werden muss, was die Wartbarkeit erhöht.
- Die Definition ist in einem Paket `isobus::UT::Q::const::AID` eingebettet, was auf eine klare Strukturierung des Codebasis hinweist.

## Zustandsübersicht

Der Baustein besitzt keinen Zustandsautomaten, da er keine ausführbare Logik enthält. Es gibt keine Zustände oder Übergänge. Die Funktionalität beschränkt sich auf die einmalige Definition von Konstanten.

## Anwendungsszenarien

Typische Anwendungsfälle für `AID_GD` sind:

- **ISOBUS-Kommunikation**: Der Baustein wird in Steuerungssystemen für landwirtschaftliche Maschinen eingesetzt, um Grafikdaten (z.B. für Display-Anzeigen) korrekt zu identifizieren und zu übertragen.
- **Einheitliche Konstantenverwaltung**: Durch die zentrale Definition von Attribut-IDs wird sichergestellt, dass alle beteiligten Softwaremodule denselben Wert für `FORMAT` verwenden, was Konsistenz und Interoperabilität fördert.
- **Konfiguration von Anzeigen**: In Anwendungen, die PNG-Grafiken auf ISOBUS-Terminals darstellen, wird `FORMAT` als Referenz für den Bildtyp genutzt.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu Funktionsblöcken (FBs), die über Ein-/Ausgänge und Verarbeitungslogik verfügen, ist `AID_GD` ein reiner Konstanten-Baustein. Ähnliche Bausteine könnten `GlobalConstants`-Definitionen für andere Attribut-IDs sein (z.B. `AID_ObjectPool`, `AID_Label` etc.), die ebenfalls nur Konstanten bereitstellen. Diese Bausteine unterscheiden sich lediglich in den definierten Konstanten und deren Bedeutung.

## Fazit

`AID_GD` ist ein einfacher, aber wichtiger Bestandteil einer ISOBUS-basierten Applikation. Er stellt die konstante Attribut-ID für Grafikdaten bereit und sorgt so für eine einheitliche und normkonforme Kommunikation. Durch die klare Definition und zentrale Bereitstellung wird die Entwicklung und Wartung von Steuerungssoftware für landwirtschaftliche Maschinen vereinfacht.
