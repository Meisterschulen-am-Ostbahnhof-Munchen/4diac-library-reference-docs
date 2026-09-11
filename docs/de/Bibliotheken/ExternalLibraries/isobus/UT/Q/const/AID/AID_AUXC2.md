# AID_AUXC2

![AID_AUXC2](./AID_AUXC2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **AID_AUXC2** ist ein GlobalConstants-Baustein der 4diac-IDE, der Konstanten für die Attribut-IDs eines "Auxiliary Control Designator Type 2" (AUX-C2) Objekts im ISOBUS (ISO 11783) Protokoll bereitstellt. Er definiert die numerischen Identifikatoren für die Pointer-Typ- und Objekt-ID-Felder, die bei der Kommunikation mit landwirtschaftlichen Geräten verwendet werden. Die bereitgestellten Konstanten sind global und stehen im gesamten System zur Verfügung.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt er keine herkömmlichen Ein-/Ausgangsports. Stattdessen werden die definierten Konstanten systemweit als globale Variablen bereitgestellt.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Der Baustein definiert zwei globale Konstanten, die die Attribut-IDs für das AUX-C2-Objekt im ISOBUS-Datenmodell festlegen. Diese Konstanten werden zur Laufzeit als unveränderliche Werte genutzt und können von anderen Bausteinen referenziert werden, um die entsprechenden Attribute eindeutig zu identifizieren.

Die definierten Konstanten sind:

- **PTR_TYPE** (USINT, Initialwert = 1): Gibt an, worauf der Pointer verweist.  
  - 0: Points to Aux Object  
  - 1: Points to Aux Function/Input  
  - 2: Points to Working Set owner of pointer  
  - 3: Points to Working Set owner of assigned function/input  

- **OBJ_ID** (USINT, Initialwert = 2): Object ID eines referenzierten Auxiliary Function- oder Auxiliary Input-Objekts oder NULL.

Diese Werte sind gemäß der ISOBUS-Spezifikation festgelegt und werden als Konstante deklariert, um versehentliche Änderungen zu vermeiden.

## Technische Besonderheiten

- **Globaler Geltungsbereich**: Die Konstanten sind im gesamten Projekt verfügbar und können von allen Bausteinen verwendet werden.
- **Unveränderlichkeit**: Da sie als `CONSTANT` deklariert sind, können sie zur Laufzeit nicht verändert werden.
- **Kompatibilität**: Der Baustein ist für die Verwendung im ISOBUS-Kontext konzipiert und erfüllt die Anforderungen der Norm ISO 11783.
- **Wartung**: Durch die zentrale Definition wird eine einheitliche Referenz auf die Attribut-IDs gewährleistet.

## Zustandsübersicht

Nicht zutreffend – der Baustein besitzt keinen internen Zustandsautomaten. Er ist ein reiner Konstantencontainer ohne dynamisches Verhalten.

## Anwendungsszenarien

- **ISOBUS-Implementierungen**: Einsatz in Steuergeräten (ECUs) für landwirtschaftliche Maschinen, die das AUX-C2-Protokoll unterstützen.
- **Konfiguration von Bediengeräten**: Definition der Pointer-Typ- und Objekt-ID-Attribute für die Zuordnung von Steuerbefehlen.
- **Entwicklung von Diagnosetools**: Verwendung als Referenz, um die Bedeutung von Attribut-IDs zu dokumentieren.

## Vergleich mit ähnlichen Bausteinen

Ähnliche GlobalConstants-Bausteine existieren für andere Attribute, z. B. **AID_AUXC1** (für Typ 1) oder **AID_AUXC3**. Diese unterscheiden sich in den Konstantenwerten und deren Semantik. AID_AUXC2 ist speziell auf den Typ 2 des Auxiliary Control Designators ausgelegt und definiert die dafür relevanten Attribut-IDs.

## Fazit

Der Baustein **AID_AUXC2** ist eine einfache, aber wichtige Komponente für die ISOBUS-Kommunikation. Er stellt die benötigten Konstanten für die AUX-C2-Attribute bereit und sorgt durch die globale Definition für eine konsistente Verwendung im gesamten System. Obwohl er keine komplexe Funktionalität besitzt, ist er für die korrekte Implementierung von ISOBUS-Anwendungen unerlässlich.
