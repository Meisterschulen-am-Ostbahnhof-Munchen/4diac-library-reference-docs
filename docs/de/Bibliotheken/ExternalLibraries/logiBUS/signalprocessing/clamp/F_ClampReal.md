# F_ClampReal

![F_ClampReal](./F_ClampReal.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **F_ClampReal** begrenzt einen eingehenden reellen Wert (`rIn`) auf einen zulässigen Bereich zwischen einem unteren Grenzwert (`rMin`) und einem oberen Grenzwert (`rMax`). Zusätzlich werden zwei boolesche Ausgangssignale ausgegeben, die anzeigen, ob der Eingangswert über- oder unterhalb des Bereichs lag. Der Baustein wird typischerweise in Signalverarbeitungsketten oder Regelungslogik eingesetzt, um unzulässige Werte zu „clampen“ und gleichzeitig eine Über- oder Unterschreitung zu signalisieren.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ** – Anforderungsereignis: Durch einen positiven Trigger an diesem Eingang wird die Verarbeitung gestartet. Die an den Daten­eingängen anliegenden Werte (`rIn`, `rMin`, `rMax`) werden übernommen und ausgewertet.

### **Ereignis-Ausgänge**

- **CNF** – Bestätigungsereignis: Nach Abschluss der Berechnung wird dieses Ereignis ausgelöst. Es signalisiert, dass die neuen Werte an den Daten­ausgängen gültig sind.

### **Daten-Eingänge**

- **rIn** (REAL): Der zu begrenzende Eingangswert.
- **rMin** (REAL): Untere Grenze des zulässigen Bereichs.
- **rMax** (REAL): Obere Grenze des zulässigen Bereichs.

### **Daten-Ausgänge**

- **rOut** (REAL, symbolisch – in der XML-Schnittstelle nicht benannt, aber als Ergebnis der Funktion verfügbar): Der auf den Bereich [rMin, rMax] begrenzte Wert.
- **xOver** (BOOL): Wird `TRUE`, wenn `rIn > rMax` – also der Eingangswert oberhalb des Maximalwerts liegt.
- **xUnder** (BOOL): Wird `TRUE`, wenn `rIn < rMin` – also der Eingangswert unterhalb des Minimalwerts liegt.

### **Adapter**

Es sind keine Adapter definiert. Der Baustein agiert vollständig über diskrete Ein-/Ausgänge.

## Funktionsweise

Beim Eintreffen des Ereignisses **REQ** wird der aktuelle Wert von `rIn` mit den Grenzen `rMin` und `rMax` verglichen:

1. **Fall: `rIn > rMax`**  
   Der Ausgang `rOut` wird auf `rMax` gesetzt. Gleichzeitig wird `xOver` auf `TRUE` und `xUnder` auf `FALSE` gesetzt.

2. **Fall: `rIn < rMin`**  
   Der Ausgang `rOut` wird auf `rMin` gesetzt. `xOver` wird `FALSE`, `xUnder` wird `TRUE`.

3. **Fall: `rMin ≤ rIn ≤ rMax`**  
   Der Eingangswert liegt innerhalb des Bereichs. `rOut` wird unverändert auf `rIn` gesetzt. Beide Flags (`xOver`, `xUnder`) werden auf `FALSE` gesetzt.

Nach Abschluss der Berechnung wird das Ereignis **CNF** ausgelöst und die neuen Ausgangswerte sind gültig. Der Baustein arbeitet deterministisch und ohne internen Zustand – er führt pro Aufruf genau eine Berechnung aus.

## Technische Besonderheiten

- Der Baustein ist als **Funktion** (FUNCTION) implementiert, nicht als ansonsten übliche Zustandsmaschine. Dadurch ist er **kombinatorisch** und besitzt keine internen Speichervariablen.
- Die Eingangsgrößen werden **mit dem Ereignis REQ** über die `With`-Zuweisung eingelesen; die Ausgangsgrößen werden beim Absenden von **CNF** mitgeliefert.
- Es findet **keine Echtzeitoptimierung** oder Sonderbehandlung für NaN/INF statt – dies ist eine reine Gleitkomma-Vergleichslogik.
- Die Flags `xOver` und `xUnder` sind sich gegenseitig ausschließend; es wird immer genau eines der beiden auf `TRUE` gesetzt, wenn der Grenzfall vorliegt.

## Zustandsübersicht

Da keine Zustandsautomatik existiert, lässt sich die gesamte Funktionalität durch drei deterministische Bedingungen beschreiben:

| Bedingung                     | rOut   | xOver | xUnder |
|-------------------------------|--------|-------|--------|
| `rIn > rMax`                  | `rMax` | TRUE  | FALSE  |
| `rIn < rMin`                  | `rMin` | FALSE | TRUE   |
| `rMin ≤ rIn ≤ rMax`           | `rIn`  | FALSE | FALSE  |

Es gibt keinen zusätzlichen Ruhezustand; der Baustein ist nur zwischen zwei Ereignissen aktiv.

## Anwendungsszenarien

- **Schutz von Aktoren**: Begrenzen eines Stellsignals auf den zulässigen Wertebereich, um mechanische oder elektrische Überlastung zu vermeiden.
- **Sensorwertaufbereitung**: Clampen von Messwerten auf einen physikalisch sinnvollen Bereich, bevor sie an Regler oder Anzeigen weitergegeben werden.
- **Plausibilitätsprüfung**: Durch die Flags `xOver` und `xUnder` kann eine übergeordnete Steuerung erkennen, wenn ein Wert außerhalb des erwarteten Bereichs liegt.
- **Regelungstechnik**: Sicherstellen, dass das Ausgangssignal eines PID-Reglers nie die Endstufen-Grenzwerte überschreitet.

## Vergleich mit ähnlichen Bausteinen

In der IEC 61131-3 gibt es die Standard-Funktionen `LIMIT` (oder `CLAMP`), die eine ähnliche Begrenzung durchführen, jedoch meist ohne zusätzliche Flag-Ausgänge. Der hier vorgestellte Baustein erweitert diese Grundfunktion um die Indikatoren `xOver` und `xUnder`. Gegenüber einer manuellen `IF-ELSE`-Programmierung bietet er eine kompakte, wiederverwendbare Lösung mit klar definierter Schnittstelle. Andere Ansätze, wie z. B. Sonderbehandlungen für ungültige Werte (NaN, INF), sind in diesem Baustein nicht implementiert, was in vielen embedded-Anwendungen ausreichend ist.

## Fazit

**F_ClampReal** ist ein einfacher, aber nützlicher Funktionsblock zur Wertebegrenzung auf eine obere und untere Grenze. Die zusätzlichen Flags ermöglichen eine einfache Überwachung, ob ein Grenzwert verletzt wurde. Aufgrund seiner kombinatorischen Natur und der schlanken Schnittstelle eignet er sich hervorragend für den Einsatz in speicher- und rechenzeitkritischen Umgebungen, wie sie in der Automatisierungstechnik häufig vorkommen.
