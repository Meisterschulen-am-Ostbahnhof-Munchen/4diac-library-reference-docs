# A2X_AUI_DEMUX_4

![A2X_AUI_DEMUX_4](./A2X_AUI_DEMUX_4.svg)

* * * * * * * * * *


## Einleitung

Der Baustein `A2X_AUI_DEMUX_4` ist ein Demultiplexer auf Basis unidirektionaler Adapter. Er leitet den am Socket `IN` anliegenden A2X-Wert an genau einen der vier Ausgänge `OUT1` bis `OUT4` weiter. Welcher Ausgang aktiviert wird, bestimmt der Socket `K`.

Besonderheit dieses Bausteins ist das änderungsbasierte Verhalten: Ein Adapter-Ausgang wird nur dann aktualisiert, wenn sich der Wert tatsächlich ändert. Nur in diesem Fall wird auch ein Ereignis an den nachgeschalteten Baustein weitergegeben. Das Ereignis `CNF` bestätigt die Übernahme des Auswahlindex `K`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Es sind keine eigenständigen Ereignis-Eingänge vorhanden. Der Baustein wird über die eingehenden Adapter-Sockets `K` und `IN` angestoßen.

### **Ereignis-Ausgänge**

| Name | Kommentar |
|------|-----------|
| `CNF` | Bestätigung der Übernahme des Auswahlindex `K`. |

### **Daten-Eingänge**

Es sind keine direkten Dateneingänge deklariert. Die Eingangsdaten werden über die Adapter-Sockets `K` und `IN` übertragen.

### **Daten-Ausgänge**

Es sind keine direkten Datenausgänge vorhanden. Die Ausgangsdaten werden ausschließlich über die Adapter-Plugs `OUT1` bis `OUT4` bereitgestellt.

### **Adapter**

| Adapter | Richtung | Typ | Kommentar |
|---------|----------|-----|-----------|
| `K` | Socket | `adapter::types::unidirectional::AUI` | Auswahlindex |
| `IN` | Socket | `adapter::types::unidirectional::A2X` | Eingangswert, der demultiplext werden soll |
| `OUT1` | Plug | `adapter::types::unidirectional::A2X` | Ausgang 1, aktiv bei `K = 0` |
| `OUT2` | Plug | `adapter::types::unidirectional::A2X` | Ausgang 2, aktiv bei `K = 1` |
| `OUT3` | Plug | `adapter::types::unidirectional::A2X` | Ausgang 3, aktiv bei `K = 2` |
| `OUT4` | Plug | `adapter::types::unidirectional::A2X` | Ausgang 4, aktiv bei `K = 3` |

## Funktionsweise

Der Baustein arbeitet als 1-zu-4-Demultiplexer. Der Wert am Socket `IN` wird abhängig vom Auswahlwert am Socket `K` auf genau einen der vier Ausgänge `OUT1` bis `OUT4` geleitet:

- `K = 0` → `OUT1`
- `K = 1` → `OUT2`
- `K = 2` → `OUT3`
- `K = 3` → `OUT4`

Wird ein neuer Wert am `IN`-Socket empfangen, prüft der Baustein, ob der Wert dem zuletzt ausgegebenen Wert des ausgewählten Ausgangs entspricht. Nur bei einer tatsächlichen Wertänderung wird der Ausgang aktualisiert und ein Ereignis über den entsprechenden A2X-Adapter ausgelöst. Nicht ausgewählte Ausgänge behalten ihren letzten Wert.

Nach der Übernahme eines neuen Auswahlindex `K` wird das Ereignis `CNF` ausgegeben.

## Technische Besonderheiten

- Der Baustein ist als generischer FB deklariert. Der generische Klassenname lautet `GEN_A2X_AUI_DEMUX`.
- Die Kommunikation erfolgt vollständig über unidirektionale Adapter; klassische Datenein- und Datenausgänge sind nicht vorhanden.
- Die Ausgänge werden nur bei Wertänderung aktualisiert. Dadurch werden unnötige Ereignisse und Verarbeitungsschritte in nachgelagerten Bausteinen vermieden.
- Die Bezeichnung `_4` kennzeichnet die Ausführung mit vier Ausgängen.
- Der Baustein ist eine A2X-Variante des `AX_AUI_DEMUX_4` und verwendet die Adapter-Typen `A2X` und `AUI`.
- Die eigentliche Logik liegt im generischen Backend `GEN_A2X_AUI_DEMUX`; die Typdefinition selbst stellt die Schnittstellen und die generische Zuordnung bereit.

## Zustandsübersicht

In der XML-Definition ist kein expliziter ECC hinterlegt. Das Verhalten lässt sich logisch wie folgt beschreiben:

| Zustand | Bedeutung |
|---------|-----------|
| Warten | Der Baustein wartet auf einen neuen Wert am Socket `K` oder `IN`. |
| Auswahl | Der Auswahlindex `K` wird ausgewertet und der zugehörige Ausgang `OUT1` bis `OUT4` bestimmt. |
| Prüfen | Der an `IN` anliegende Wert wird mit dem aktuellen Wert des ausgewählten Ausgangs verglichen. |
| Aktualisieren | Bei Wertänderung wird der A2X-Ausgang aktualisiert und das Ereignis ausgelöst. |
| Bestätigen | `CNF` signalisiert die erfolgreiche Übernahme des Auswahlindex `K`. |

## Anwendungsszenarien

- Verteilung eines A2X-Signals an mehrere Regler oder Aktoren, wobei nur ein Empfänger zur gleichen Zeit aktiv ist.
- Betriebsarten-Umschaltung in modularen Automatisierungsanlagen: Ein Sollwert wird je nach Modus an das aktive Modul geroutet.
- Diagnose- und Monitoring-Systeme: Ein Messwert wird wahlweise an Anzeige, Speicher oder Alarmauswertung geleitet.
- Ereignisarme Systeme, in denen die Kommunikationslast durch ausschließliche Aktualisierung bei Wertänderungen reduziert werden soll.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Eigenschaft |
|----------|-------------|
| `A2X_AUI_DEMUX_4` | Vier A2X-Ausgänge, Auswahl über AUI, ereignisoptimierte Wertänderung |
| `AX_AUI_DEMUX_4` | Ursprüngliche Variante mit `AX`-Adaptern; `A2X_AUI_DEMUX_4` ist die A2X-basierte Weiterentwicklung |
| Konventioneller Demultiplexer | Besitzt direkte Daten-Ein- und Ausgänge; kein Adapterkonzept und keine änderungsbasierte Eventweiterleitung |


## Fazit

`A2X_AUI_DEMUX_4` ist ein flexibler und ereignisoptimierter Adapter-Demultiplexer für vier Ausgänge. Er eignet sich besonders für modulare IEC-61499-Systeme, in denen ein Wert wahlweise an mehrere Senken verteilt werden soll und unnötige Ereignisse vermieden werden müssen. Durch die generische Definition und die Verwendung unidirektionaler Adapter ist er gut wiederverwendbar und in die 4diac-IDE integrierbar.
