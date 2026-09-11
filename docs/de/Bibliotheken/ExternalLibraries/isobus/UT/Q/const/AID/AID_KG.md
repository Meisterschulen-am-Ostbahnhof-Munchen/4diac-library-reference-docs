# AID_KG

![AID_KG](./AID_KG.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_KG` (Attribute Identifier – Key Group) ist ein globaler Konstantencontainer innerhalb des ISOBUS-Anwendungskontexts. Er stellt numerische Konstanten bereit, die als Attributed-IDs für Schlüsselgruppen (Key Groups) verwendet werden. Diese Konstanten ermöglichen eine standardisierte Identifikation von Objekteigenschaften wie Optionen und Namen, wodurch die Interoperabilität zwischen verschiedenen ISOBUS-Komponenten erleichtert wird.

## Schnittstellenstruktur

Da `AID_KG` als reine Konstantensammlung definiert ist, besitzt es keine klassischen Schnittstellen eines Funktionsblocks. Es werden weder Ereignisse noch Datenports oder Adapter bereitgestellt. Die Struktur eines solchen Containers besteht ausschließlich aus den deklarierten Konstanten.

### **Ereignis-Eingänge**

Keine vorhanden – der Baustein enthält keine Ereignis-Eingänge.

### **Ereignis-Ausgänge**

Keine vorhanden – der Baustein enthält keine Ereignis-Ausgänge.

### **Daten-Eingänge**

Keine vorhanden – der Baustein besitzt keine Dateneingangsports.

### **Daten-Ausgänge**

Keine vorhanden – der Baustein besitzt keine Datenausgangsports.

### **Adapter**

Keine vorhanden – der Baustein enthält keine Adapter.

## Funktionsweise

Der Baustein definiert zwei globale Konstanten, die in anderen Bausteinen oder Systemen referenziert werden können:

- **`OPTIONS`**: Der Wert `USINT#1` (dezimal 1) repräsentiert die Attributed-ID `AID_KG_OPTIONS`. Diese ID ist als Bitmaske definiert, bei der Bit 0 die Verfügbarkeit (Available) und Bit 1 die Transparenz (Transparent) kennzeichnet. Sie wird verwendet, um Optionen einer Schlüsselgruppe zu identifizieren.
- **`NAME`**: Der Wert `USINT#2` (dezimal 2) steht für `AID_KG_NAME` und referenziert ein Objekt (z. B. eine Ausgangszeichenkette oder einen Objektzeiger), das den Namen der Schlüsselgruppe enthält.

Die Konstanten sind als `USINT` (Unsigned Short Integer) typisiert und besitzen feste Initialwerte, die während der Laufzeit nicht geändert werden können. Durch die zentrale Definition werden Doppelbelegungen und Inkonsistenzen vermieden.

## Technische Besonderheiten

- **Typisierung**: Die Konstanten sind vom Typ `USINT`, also 8-Bit-Ganzzahlen ohne Vorzeichen, was eine effiziente Speichernutzung ermöglicht.
- **Bitmaskenlogik**: Die Konstante `OPTIONS` verwendet eine Bitmaske, um mehrere Eigenschaften in einem einzigen Wert zu kodieren – Bit 0 für "Verfügbar" und Bit 1 für "Transparent". Diese Kodierung erlaubt eine kompakte Repräsentation und einfache Abfragen.
- **Initialisierung**: Die Initialwerte sind direkt im Deklarationstext hinterlegt (`USINT#1` bzw. `USINT#2`), sodass beim Laden des Bausteins keine zusätzliche Initialisierungssequenz erforderlich ist.
- **Namensgebung**: Die Konstantenbezeichnungen folgen einem präfixierten Schema (`AID_KG_...`), was die Zuordnung zur Schlüsselgruppe erleichtert.

## Zustandsübersicht

Da `AID_KG` kein funktionaler Baustein mit einem Zustandsautomaten ist, existiert kein expliziter Zustandsübergang. Die Konstanten sind statisch und verändern ihren Wert während der Laufzeit nicht. Der Baustein kann als "permanent aktiv" betrachtet werden, ohne dass ein Zustandsmanagement erforderlich ist.

## Anwendungsszenarien

- **ISOBUS-Implementierungen**: Der Baustein wird in ISOBUS-Anwendungen eingesetzt, um Schlüsselgruppen-Attribute zu identifizieren. Beispielsweise dient `NAME` dazu, den Namen einer Schlüsselgruppe an eine Anzeige oder ein Bedienfeld zu übergeben.
- **Konfigurationssysteme**: Bei der Konfiguration von Maschinensteuerungen können die Konstanten verwendet werden, um auf einheitliche Attribut-IDs zu verweisen, ohne magische Zahlen im Code zu verwenden.
- **Interoperabilität**: Durch die zentrale Definition können verschiedene Module eines Systems die gleichen IDs verwenden und so konsistente Datenstrukturen sicherstellen.

## Vergleich mit ähnlichen Bausteinen

Andere Konstantencontainer wie `AID_COMM`, `AID_COM` oder `AID_OBJ` (nicht im Lieferumfang) folgen typischerweise demselben Muster: Sie definieren numerische IDs für verschiedene Objekttypen (z. B. Kommunikation, gemeinsame Objekte). Im Gegensatz dazu fokussiert `AID_KG` speziell auf Schlüsselgruppen-Attribute. Ein direkter Vergleich zeigt, dass die Struktur aller dieser Container ähnlich ist – sie bestehen aus globalen Konstanten ohne Schnittstellen –, jedoch unterschiedliche Satellitenbezeichnungen verwenden, um die jeweiligen Bereiche abzudecken.

## Fazit

Der Baustein `AID_KG` ist ein schlanker, aber essenzieller Bestandteil für ISOBUS-basierte Systeme. Er definiert klar benannte und typisierte Konstanten für Schlüsselgruppen-Attribute und reduziert dadurch die Fehleranfälligkeit bei der Verwendung numerischer IDs. Obwohl er keine dynamischen Funktionen besitzt, trägt er durch seine statische Natur zur Robustheit und Wartbarkeit des Gesamtsystems bei. Die klare Struktur und die dokumentierte Bedeutung der Konstanten machen ihn zu einem verlässlichen Baustein für Entwickler und Integratoren.
