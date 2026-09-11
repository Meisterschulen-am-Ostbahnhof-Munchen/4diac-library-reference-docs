# AID_SK

![AID_SK](./AID_SK.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_SK` (Attribute IDs für Soft Keys) definiert globale Konstanten zur Identifikation von Attributen eines Soft-Key-Objekts im ISOBUS-Virtual-Terminal (VT). Diese Konstanten werden verwendet, um auf Attribute wie Hintergrundfarbe oder Tastencode zu referenzieren. Der Baustein ist Teil des Pakets `isobus::UT::Q::const::AID` und folgt der IEC 61499-1 Norm.

## Schnittstellenstruktur

Der Baustein besitzt keine Ereignis- oder Daten-Ein-/Ausgänge, da es sich um eine reine Konstantendefinition handelt. Es werden keine Adapter bereitgestellt.

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

Der Baustein stellt zwei globale Konstanten vom Typ `USINT` (Unsigned Short Integer) bereit:

- `BACKGROUND_COLOUR` mit dem Wert `1` – entspricht dem Attribut `AID_SK_BACKGROUND_COLOUR` (Hintergrundfarbindex).
- `KEY_CODE` mit dem Wert `2` – entspricht dem Attribut `AID_SK_KEY_CODE` (Tastencode, den das VT in der Soft-Key-Aktivierungsnachricht meldet).

Diese Konstanten können in anderen Bausteinen oder Programmen verwendet werden, um auf die entsprechenden Attribut-IDs zuzugreifen, ohne magische Zahlen im Code zu verwenden.

## Technische Besonderheiten

- Die Konstanten sind als `VAR_GLOBAL CONSTANT` deklariert und daher zur Laufzeit unveränderlich.
- Die Werte sind als `USINT`-Datentyp definiert und initialisiert.
- Das Original stammt aus einer ISOBUS-Implementierung und ist durch die Eclipse Public License 2.0 geschützt.
- Die Konstanten sind in einem Compiler-Paket (`isobus::UT::Q::const::AID`) organisiert, was die Wiederverwendung in verschiedenen Modulen ermöglicht.

## Zustandsübersicht

Da es sich um reine Konstanten handelt, besitzt der Baustein keinen Zustandsautomaten. Die Werte sind statisch und ändern sich während der Laufzeit nicht.

## Anwendungsszenarien

- **Soft-Key-Objekt konfigurieren**: Bei der Erstellung eines Soft-Key-Objekts im VT können die Attribut-IDs verwendet werden, um die Hintergrundfarbe oder den Tastencode zu setzen.
- **Soft-Key-Aktivierung auswerten**: Beim Empfang einer Soft-Key-Aktivierungsnachricht wird über die Konstante `KEY_CODE` der zugehörige Tastencode identifiziert.
- **LESEN/SETZEN von Objektattributen**: In Kommunikationsprotokollen, die auf ISOBUS basieren, können diese IDs als Parameter für Objekt-Attribut-Operationen genutzt werden.

## Vergleich mit ähnlichen Bausteinen

Es gibt weitere globale Konstanten-Bausteine für verschiedene Objekttypen (z.B. `AID_OBJ` für allgemeine Objekte). `AID_SK` fokussiert spezifisch auf Soft-Key-Objekte. Im Gegensatz zu Funktionsblöcken, die Verhalten kapseln, bieten diese Konstanten nur eine Wertequelle ohne Logik.

## Fazit

Der Baustein `AID_SK` stellt eine saubere und standardisierte Möglichkeit dar, die Attribut-IDs für Soft-Key-Objekte im ISOBUS-Kontext zu verwenden. Durch die zentrale Definition werden Fehler vermieden und die Wartung erleichtert. Er ist ein typisches Beispiel für Konstanten-Bausteine in IEC 61499-Systemen.
