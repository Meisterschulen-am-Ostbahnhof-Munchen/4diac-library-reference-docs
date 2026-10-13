# AX_BLINK

![AX_BLINK_network](./AX_BLINK_network.svg)

* * * * * * * * * *

## Einleitung

Der AX_BLINK ist ein Funktionsblock, der ein blinkendes Signal über einen AX-Adapter ausgibt.

![AX_BLINK](AX_BLINK.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **START**: Startet die periodische Erzeugung von Ereignissen; trägt `TIMELOW` und `TIMEHIGH`.
- **STOP**: Stoppt die Erzeugung von Ereignissen.

### **Daten-Eingänge**

- **TIMELOW** (TIME): Zeit, in der OUT FALSE ist.
- **TIMEHIGH** (TIME): Zeit, in der OUT TRUE ist.

### **Adapter**

**Plugs (Ausgänge):**

- **OUT** (adapter::types::unidirectional::AX): Ausgabewert, beginnt mit FALSE und wechselt zwischen TRUE und FALSE entsprechend den vorgegebenen High- und Low-Zeiten.

## Funktionsweise

Der Baustein schaltet den Ausgang OUT nach `START` abwechselnd für die Zeit `TIMELOW` aus (FALSE) und für die Zeit `TIMEHIGH` ein (TRUE), bis `STOP` eintrifft.

## Technische Besonderheiten

- Verwendet unidirektionale Adapter.

## Zustandsübersicht

Zustandsbasiert (Blinken).

## Anwendungsszenarien

Blinkende Anzeigen.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- **E_BLINK**: Standard Blink-Baustein.

## 🛠️ Zugehörige Übungen

- [Uebung_020f3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f3_AX/)

## Fazit

Adapter-basierter Blink-Baustein.
