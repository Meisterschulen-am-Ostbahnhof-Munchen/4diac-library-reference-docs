# AX_E_PERMIT_INVERT_2


![AX_E_PERMIT_INVERT_2_network](./AX_E_PERMIT_INVERT_2_network.svg)

![AX_E_PERMIT_INVERT_2](./AX_E_PERMIT_INVERT_2.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock **AX_E_PERMIT_INVERT_2** ist eine Subapplikation, die zwei Ereignis-Eingänge über ein invertiertes Freigabesignal auf zwei Ereignis-Ausgänge durchschaltet. Er kombiniert dabei einen Inverter (AX_NOT_INIT) mit einem 2‑kanaligen Ereignis-Freigabe-Gate (AX_E_PERMIT_2). Die Subapplikation erlaubt es, Ereignisse nur dann weiterzuleiten, wenn das über den Adapter `PERMIT` anliegende Freigabesignal **inaktiv** ist. Dadurch eignet sie sich für Logiken, bei denen eine fehlende Freigabe (z. B. bei Sicherheitsfunktionen) die Ereignisweiterleitung blockiert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ   | Kommentar               |
|------|-------|-------------------------|
| EI1  | Event | Event input channel 1   |
| EI2  | Event | Event input channel 2   |

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar                |
|------|-------|--------------------------|
| EO1  | Event | Event output channel 1   |
| EO2  | Event | Event output channel 2   |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Name   | Typ                                     | Kommentar                        |
|--------|-----------------------------------------|----------------------------------|
| PERMIT | adapter::types::unidirectional::AX      | Invertiertes Freigabesignal      |

Der Adapter `PERMIT` ist ein unidirektionaler Eingangsadapter, der das Freigabesignal als booleschen Wert liefert.

## Funktionsweise

Die Subapplikation verbindet intern die beiden Ereignis-Eingänge `EI1` und `EI2` direkt mit den entsprechenden Eingängen des Bausteins `AX_E_PERMIT_2`. Dieser Baustein leitet ein Ereignis genau dann von seinem Eingang `EIx` zu seinem Ausgang `EOx` weiter, wenn sein Freigabesignal `PERMIT` aktiv (logisch 1) ist.

Das Freigabesignal der Subapplikation wird jedoch **vor** der Verwendung durch den Inverter `AX_NOT_INIT` logisch negiert. Dadurch dreht sich die Freigabelogik um:

- Ist das äußere `PERMIT`-Signal **aktiv** (Wert = TRUE), dann wird es nach der Invertierung zu FALSE. Das Gate `AX_E_PERMIT_2` sperrt die Ereignisweiterleitung.
- Ist das äußere `PERMIT`-Signal **inaktiv** (Wert = FALSE), dann wird es nach der Invertierung zu TRUE. Das Gate lässt die Ereignisse von `EI1` zu `EO1` und von `EI2` zu `EO2` durch.

Somit fungiert die Subapplikation als **invertiertes Freigabegate**: Ereignisse werden nur dann weitergegeben, wenn das Freigabesignal nicht ansteht.

## Technische Besonderheiten

- **Kapselung**: Die interne Verdrahtung ist in der Subapplikation verborgen; nach außen erscheint nur die Schnittstelle mit den Ereignissen und dem Adapter.
- **Keine Datenwege**: Es existieren keine Daten-Ein-/Ausgänge – die Subapplikation arbeitet ausschließlich mit Ereignissen und booleschen Werten über den Adapter.
- **Adaptertyp**: Der Adapter `PERMIT` ist vom Typ `adapter::types::unidirectional::AX`, ein unidirektionaler Eingangsadapter, der für reine Signalübertragung ohne Rückkanal ausgelegt ist.
- **Zwei Kanäle**: Die Ereignispaare (EI1/EO1, EI2/EO2) sind unabhängig, werden aber gemeinsam durch das Freigabesignal gesteuert.

## Zustandsübersicht

Die Subapplikation selbst besitzt keinen expliziten Zustand, da sie ausschließlich aus kombinatorischen und ereignisgesteuerten Funktionen besteht. Die interne Funktionalität lässt sich wie folgt beschreiben:

| Zustand / Bedingung            | Verhalten                                      |
|--------------------------------|------------------------------------------------|
| `PERMIT` = FALSE               | Ereignisse an EI1/EI2 werden an EO1/EO2 durchgereicht |
| `PERMIT` = TRUE                | Ereignisse an EI1/EI2 werden blockiert         |

Ein definierter Initialzustand ist nicht vorhanden; das Verhalten hängt ausschließlich vom aktuellen Wert des Adapters ab.

## Anwendungsszenarien

- **Sicherheitslogik**: Durchschalten von Ereignissen nur dann, wenn ein Freigabesignal **nicht** aktiv ist (z. B. bei Not-Aus-Pfaden, bei denen Ereignisse bei aktiver Störung unterdrückt werden).
- **Invertierte Sperre**: In Automatisierungsanlagen, wo Ereignisse (z. B. Taktimpulse) nur bei abgeschalteter Freigabe verarbeitet werden sollen.
- **Test-Szenarien**: Bei der Simulation von Fehlerzuständen, bei denen das Fehlen eines Signals die normale Verarbeitung erlaubt.

## Vergleich mit ähnlichen Bausteinen

| Baustein          | Beschreibung                                                                    | Unterschied                                                        |
|-------------------|---------------------------------------------------------------------------------|--------------------------------------------------------------------|
| `AX_E_PERMIT_2`   | 2‑Kanal Freigabegate: Ereignisse werden bei **aktivem** Freigabesignal durchgelassen. | `AX_E_PERMIT_INVERT_2` invertiert das Freigabesignal, sodass bei inaktivem Signal durchgeschaltet wird. |
| `AX_E_PERMIT_1`   | 1‑Kanal Variante ohne Invertierung                                              | Nur ein Ereigniskanal, keine Invertierung.                         |
| `AX_NOT_INIT`     | Wandler für boolesche Signale mit Invertierung                                  | Nur als Einzelbaustein für logische Negation; keine Ereignissteuerung. |

## Fazit

Der Funktionsblock **AX_E_PERMIT_INVERT_2** stellt eine kompakte, wiederverwendbare Lösung für eine invertierte, 2‑kanalige Ereignisfreigabe dar. Durch die Kombination eines Inverters mit einem standardisierten Freigabegate wird eine klare und robuste Steuerungslogik erreicht. Die weglassung von Daten-Ein-/Ausgängen und die reine Ereignisorientierung machen ihn ideal für ereignisbasierte Automatisierungssysteme, die eine präzise Freigabesteuerung benötigen.