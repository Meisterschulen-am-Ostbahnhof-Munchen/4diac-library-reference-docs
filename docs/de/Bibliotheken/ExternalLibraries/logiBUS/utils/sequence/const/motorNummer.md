# motorNummer

![motorNummer](./motorNummer.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `motorNummer` ist eine globale Konstanten-Definition (GlobalConstants) im 4diac-IDE-Umfeld. Er stellt symbolische Namen für die Identifikation verschiedener Motoren in einer Anlagensteuerung bereit. Die Werte sind vom Typ `SINT` (signed short integer) und decken den Bereich von 0 bis 6 ab, wobei 0 für „kein Motor“ steht und 1 bis 6 den Motoren M1 bis M6 zugeordnet sind. Diese Konstanten werden typischerweise in Sequenzsteuerungen oder anderen Anwendungen verwendet, um Motor-IDs lesbar und wartbar zu definieren.

Die Definition ist unter dem Paket `logiBUS::utils::sequence::const` abgelegt und wird im Rahmen des Projekts „AnlagenSequenz“ genutzt, wie aus dem Kommentar hervorgeht.

## Schnittstellenstruktur

Da es sich um eine reine Konstantendefinition handelt, besitzt der Baustein **keine** Ein-/Ausgangsschnittstellen im Sinne von Ereignissen, Daten oder Adaptern. Die Schnittstellenstruktur entfällt vollständig.

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

Der Baustein `motorNummer` definiert sieben globale Konstanten, die innerhalb des 4diac-Projekts (z. B. in ST- oder FBD-Programmen) referenziert werden können. Durch die Verwendung symbolischer Namen anstelle von magischen Zahlen wird die Lesbarkeit und Wartbarkeit des Codes erhöht. Beispielsweise kann in einer Sequenz ein Übergang `TRANSIT_MOTOR` auf die Konstante `MOTOR_M3` verweisen, um den dritten Motor anzusteuern.

Die Konstanten sind als `CONSTANT` deklariert und können zur Laufzeit nicht verändert werden. Sie sind global gültig, sobald der Baustein in das Projekt eingebunden ist.

## Technische Besonderheiten

- **Typ:** `SINT` – vorzeichenbehafteter 8‑Bit‑Integer (Wertebereich −128 bis 127).
- **Initialwerte:** Die Konstanten sind mit expliziten Werten initialisiert (z. B. `SINT#0`, `SINT#1`, …).
- **Namensgebung:** Die Bezeichnungen folgen einem einheitlichen Muster: `MOTOR_KEINER`, `MOTOR_M1` bis `MOTOR_M6`.
- **Lizenz:** Die Software ist unter der Eclipse Public License 2.0 (EPL‑2.0) veröffentlicht, wie im Identifikationsblock angegeben.
- **Autor:** Franz Höpfinger, Firma HR Agrartechnik GmbH, Datum: 2026‑08‑25.
- **Paket:** `logiBUS::utils::sequence::const` – gruppiert die Konstanten thematisch.

## Zustandsübersicht

Ein Zustandsübersicht ist für einen Konstantendefinitions-Baustein nicht anwendbar, da keine endlichen Zustandsautomaten oder dynamischen Verhalten existieren.

## Anwendungsszenarien

- **Steuerung einer Anlagensequenz:** Die Konstanten werden verwendet, um in Übergangsbedingungen (z. B. `TRANSIT_MOTOR`) die aktive Motor-ID zu vergleichen.
- **Parametrierung:** Anstelle von Zahlenwerten in Funktionsblöcken kann auf die symbolischen Namen zugegriffen werden, was Fehler bei der Eingabe vermeidet.
- **Projektweite Wiederverwendung:** Da die Konstanten global sind, können sie in verschiedenen FB-Instanzen und Programmteilen ohne erneute Definition verwendet werden.

## Vergleich mit ähnlichen Bausteinen

In IEC 61499 gibt es keine standardisierten GlobalConstants-Bausteine; die Definition ist eine Erweiterung der 4diac-IDE. Alternativ könnten Konstanten als lokale Variablen in jedem Baustein deklariert werden, was jedoch die Konsistenz erschwert. Die zentrale Definition in einem eigenen Baustein wie `motorNummer` bietet eine klare und zentrale Wartungsmöglichkeit. Andere Bausteine könnten ähnliche Konstanten für andere Geräte (z. B. Pumpen, Ventile) definieren, sodass das Muster wiederverwendbar ist.

## Fazit

`motorNummer` ist ein einfacher, aber effektiver Baustein zur zentralen Verwaltung von Motor-IDs als globale Konstanten. Er trägt zur Codequalität bei, indem er magische Zahlen durch sprechende Namen ersetzt und so die Lesbarkeit und Wartbarkeit der Steuerungslogik verbessert. Die klare Struktur und die Verwendung von `SINT` machen ihn für die vorgesehene Anwendung in der Anlagensteuerung geeignet. Aufgrund fehlender Schnittstellen ist er prototypisch für Konstantendefinitionen in 4diac und kann als Vorlage für ähnliche Konstantenblöcke dienen.
