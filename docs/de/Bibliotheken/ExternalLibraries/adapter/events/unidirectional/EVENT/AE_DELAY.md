# AE_DELAY

![AE_DELAY](./AE_DELAY.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **AE_DELAY** ist ein Wrapper für den Standard-IEC-61499-Baustein `E_DELAY`, der speziell für die Verwendung mit **Event-Adaptern (AE)** konzipiert wurde. Er ermöglicht die verzögerte Weiterleitung von Ereignissen innerhalb einer adapterbasierten Architektur. Der Parameter `PT` (Preset Time) wird über den Adapter-Socket `PT` (Typ `ATM`) zugeführt.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine direkten Ereignis-Eingänge. Alle Ereignisse werden über Adapter empfangen.

### **Ereignis-Ausgänge**

Keine direkten Ereignis-Ausgänge. Alle Ausgangsereignisse werden über den Adapter `EO` geleitet.

### **Daten-Eingänge**

Keine direkten Daten-Eingänge. Die Verzögerungszeit wird über den ATM-Adapter-Socket `PT` zugeführt.

### **Daten-Ausgänge**

Keine Daten-Ausgänge.

### **Adapter**

| Name      | Typ                                | Art              | Kommentar                                                          |
| :-------- | :--------------------------------- | :--------------- | :----------------------------------------------------------------- |
| **START** | adapter::types::unidirectional::AE | Socket (Eingang) | Startet die Zeitverzögerung (löst intern `START` aus).             |
| **STOP**  | adapter::types::unidirectional::AE | Socket (Eingang) | Stoppt/bricht die Zeitverzögerung ab (löst intern `STOP` aus).     |
| **PT**    | adapter::types::unidirectional::ATM| Socket (Eingang) | Preset Time (Delay-Zeit als ATM-Adapter, sauber initialisierbar).  |
| **EO**    | adapter::types::unidirectional::AE | Plug (Ausgang)   | **Event Output**: Gibt das Ereignis nach Ablauf der Zeit `PT` aus. |

## Funktionsweise

Der **AE_DELAY** Baustein agiert als Brücke zwischen der Adapter-Welt und dem klassischen `E_DELAY` Timer:

1. **Starten des Timers:** Trifft ein Ereignis am Adapter-Socket **START** ein, wird dieses intern an den `START`-Eingang des eingebetteten `E_DELAY` Bausteins weitergeleitet.
2. **Ablauf der Zeit:** Sobald die Zeit an **PT** verstrichen ist, generiert der interne Timer ein Ereignis an **EO**.
3. **Abbrechen:** Trifft ein Ereignis am Adapter-Socket **STOP** ein, wird der laufende Timer sofort unterbrochen.
