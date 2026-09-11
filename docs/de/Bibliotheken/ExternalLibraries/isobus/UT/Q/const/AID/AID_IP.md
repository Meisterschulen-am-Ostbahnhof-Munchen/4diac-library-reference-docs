# AID_IP

![AID_IP](./AID_IP.svg)

* * * * * * * * * *
## Einleitung
Der Baustein `AID_IP` ist ein globaler Konstanten-Baustein (GlobalConstants) zur Definition von Attribut-IDs für Eingabeobjekte im ISOBUS-Kontext. Er stellt eine einzige Konstante `VALIDATION_TYPE` bereit, die den Validierungstyp für Eingabeattribute festlegt. Diese Konstante wird typischerweise in Client- oder Server-Komponenten verwendet, um zu bestimmen, ob bei der Validierung von Zeichenlisten gültige oder ungültige Zeichen aufgeführt werden.

## Schnittstellenstruktur
Da `AID_IP` ein GlobalConstants-Baustein ist, besitzt er keine Ereignis- oder Dateneingänge/-ausgänge im Sinne eines Funktionsblocks. Es sind keine Adapter vorhanden.

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

### **Globale Konstanten**
| Konstante            | Typ    | Initialwert | Kommentar                                                                                     |
|----------------------|--------|-------------|-----------------------------------------------------------------------------------------------|
| `VALIDATION_TYPE`    | `USINT`| `USINT#1`   | 1: AID_IP_VALIDATION_TYPE – Validierungstyp: 0 = gültige Zeichen sind gelistet, 1 = ungültige Zeichen sind gelistet. |

## Funktionsweise
Der Baustein stellt eine globale Konstante bereit, die überall im Projekt referenziert werden kann. Die Konstante `VALIDATION_TYPE` wird als `USINT` (Unsigned Short Integer) mit dem Wert `1` initialisiert. Gemäß Kommentar bedeutet der Wert `1`, dass bei einer Validierung die Liste der **ungültigen** Zeichen angegeben wird. Ein Wert `0` würde bedeuten, dass gültige Zeichen aufgeführt sind.

Die Konstante ist als `VAR_GLOBAL CONSTANT` definiert und besitzt damit im gesamten Anwendungsbereich Gültigkeit. Sie kann in FB-Implementierungen oder anderen Bausteinen per Namen angesprochen werden, ohne dass eine Instanziierung erforderlich ist.

## Technische Besonderheiten
- Der Typ `USINT` entspricht einer 8-Bit-Ganzzahl ohne Vorzeichen.
- Der Initialwert wird explizit als `USINT#1` gesetzt, was eine typsichere Initialisierung darstellt.
- Der Baustein enthält keine Logik oder Zustände; er dient rein der Bereitstellung von Konstanten für die ISOBUS-Objektattribut-Verarbeitung.
- Der Paketname `isobus::UT::Q::const::AID` deutet auf eine hierarchische Struktur innerhalb eines ISOBUS-Frameworks hin.

## Zustandsübersicht
Nicht zutreffend – dieser Baustein besitzt keine Zustände.

## Anwendungsszenarien
- **Validierung von Eingabeattributen:** In ISOBUS-Datenbanken müssen Eingabefelder (z.B. für Benutzereingaben) validiert werden. Mit `VALIDATION_TYPE` kann festgelegt werden, ob die übermittelte Zeichenliste gültige oder ungültige Zeichen enthält.
- **Parametrierung von Client-Objekten:** Wenn ein Client ein Eingabeobjekt mit Attribut-IDs konfiguriert, kann er die Konstante nutzen, um den Validierungsmodus festzulegen.
- **Einheitliche Konfiguration:** Durch die zentrale Definition wird vermieden, dass Magische Zahlen im Code auftauchen; Änderungen können an einer Stelle vorgenommen werden.

## Vergleich mit ähnlichen Bausteinen
Im ISOBUS-Kontext existieren weitere GlobalConstants-Bausteine für andere Objekttypen (z.B. `AID_*` für verschiedene Attribut-IDs). Diese Bausteine folgen dem gleichen Muster: Sie definieren Konstanten für spezifische Parameter. `AID_IP` ist speziell auf Eingabeobjekte (Input) ausgerichtet und stellt den Validierungstyp bereit. Andere Bausteine können z.B. Attribut-IDs für Sichtbarkeit, Editierbarkeit oder Formatierungen enthalten, ohne dass sie sich in der grundlegenden Funktionsweise unterscheiden.

## Fazit
Der Baustein `AID_IP` ist ein einfacher, aber wichtiger Bestandteil zur Vereinheitlichung von ISOBUS-Konstanten. Er definiert den Validierungstyp für Eingabeattribute und trägt so zur Klarheit und Wartbarkeit des Codes bei. Durch die Verwendung globaler Konstanten wird die Konfiguration zentralisiert und erleichtert die Anpassung an unterschiedliche Anforderungen.