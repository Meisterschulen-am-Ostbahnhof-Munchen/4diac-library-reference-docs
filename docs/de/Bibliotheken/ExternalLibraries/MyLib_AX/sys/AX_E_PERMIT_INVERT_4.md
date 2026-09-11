# AX_E_PERMIT_INVERT_4


![AX_E_PERMIT_INVERT_4_network](./AX_E_PERMIT_INVERT_4_network.svg)

![AX_E_PERMIT_INVERT_4](./AX_E_PERMIT_INVERT_4.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock (SubApp) **AX_E_PERMIT_INVERT_4** realisiert ein 4‑Kanal‑Event‑Freigabe‑Gate mit invertierter Freigabelogik. Er kombiniert die Bausteine `AX_NOT_INIT` und `AX_E_PERMIT_4` zu einer kompakten Einheit, die über einen Adapter gesteuert wird. Die vier Event‑Eingänge werden nur dann auf die jeweiligen Ausgänge durchgeschaltet, wenn das Freigabesignal **inaktiv** (FALSE) ist – eine logische Invertierung des üblichen Freigabe‑Verhaltens.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| EI1  | Event | Event‑Eingang Kanal 1 |
| EI2  | Event | Event‑Eingang Kanal 2 |
| EI3  | Event | Event‑Eingang Kanal 3 |
| EI4  | Event | Event‑Eingang Kanal 4 |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| EO1  | Event | Event‑Ausgang Kanal 1 |
| EO2  | Event | Event‑Ausgang Kanal 2 |
| EO3  | Event | Event‑Ausgang Kanal 3 |
| EO4  | Event | Event‑Ausgang Kanal 4 |

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Name | Typ | Kommentar |
|------|-----|-----------|
| PERMIT | adapter::types::unidirectional::AX | Invertiertes Freigabesignal (Socket) |

## Funktionsweise

Die SubApp `AX_E_PERMIT_INVERT_4` besteht intern aus zwei miteinander verbundenen Funktionsbausteinen:

1. **AX_NOT_INIT** – invertiert das über den Adapter `PERMIT` eingehende Freigabesignal.
2. **AX_E_PERMIT_4** – nutzt das invertierte Signal als Freigabe für vier parallele Event‑Kanäle.

Die Adapterverbindung stellt sicher, dass das am `PERMIT`‑Socket anliegende Signal zuerst invertiert wird, bevor es auf den Freigabeeingang des Event‑Tores gelangt. Dadurch ergibt sich folgende Logik:

- Wenn `PERMIT = TRUE` → Invertiertes Signal = `FALSE` → Alle vier Event‑Kanäle sind **gesperrt**.
- Wenn `PERMIT = FALSE` → Invertiertes Signal = `TRUE` → Alle vier Event‑Kanäle sind **durchgeschaltet**.

Ereignisse an `EI1` bis `EI4` werden also nur dann zu den entsprechenden Ausgängen `EO1` bis `EO4` weitergeleitet, wenn das Freigabesignal am Adapter NICHT aktiv ist.

## Technische Besonderheiten

- Die SubApp verwendet Adapter‑Typen (`unidirectional::AX`), die eine lose Kopplung zwischen den Bausteinen ermöglichen.
- Die Invertierung und die Freigabelogik sind in einer einzigen SubApp gekapselt, was die Wiederverwendung und Wartung erleichtert.
- Es sind keine zeitlichen Verzögerungen oder internen Zustandsmaschinen vorhanden – die Durchschaltung erfolgt rein ereignis‑ und signalabhängig.
- Die Anordnung der internen Bausteine (x, y‑Koordinaten) ist für die Netzwerk‑Ausführung irrelevant, lediglich die Verdrahtung ist entscheidend.

## Zustandsübersicht

Die SubApp selbst besitzt keinen internen Zustand. Der Durchlasszustand der vier Kanäle hängt ausschließlich vom aktuellen Wert des Adaptersignals `PERMIT` ab. Der effektive Zustand lässt sich wie folgt beschreiben:

| Adapter PERMIT | Invertiertes Signal | Kanalverhalten |
|----------------|---------------------|----------------|
| TRUE           | FALSE               | Events werden blockiert |
| FALSE          | TRUE                | Events werden durchgelassen |

## Anwendungsszenarien

- **Sicherheitslogik**: Freigabe für den Normalbetrieb, Sperre bei aktivem Alarmsignal.
- **NOT‑Aus‑Logik**: Maschinen‑ oder Prozesssteuerung, bei der Ereignisse nur bei fehlendem Freigabesignal verarbeitet werden dürfen.
- **Datenfluss‑Steuerung**: Selektives Durchschalten von Event‑Strömen in Abhängigkeit von einer invertierten Bedingung.
- **Prototypen‑Entwicklung**: Einfache Erweiterung vorhandener Freigabe‑Logiken durch Hinzufügen eines `NOT`‑Gliedes.

## Vergleich mit ähnlichen Bausteinen

- **AX_E_PERMIT_4** (ohne Invertierung): Erlaubt die Durchschaltung bei `PERMIT = TRUE`. `AX_E_PERMIT_INVERT_4` kehrt diese Logik um.
- **AX_NOT_INIT + AX_E_PERMIT_4** als separate Bausteine: Bietet dieselbe Funktionalität, erfordert aber eine manuelle Verkabelung. Die SubApp kapselt diese Kombination und vereinfacht die Einbindung in bestehende Systeme.
- **NICHT‑Gatter** (rein boolesch): Ohne Event‑Verarbeitung möglich, aber diese SubApp kombiniert Invertierung mit Event‑Gating in einem Baustein.

## Fazit

`AX_E_PERMIT_INVERT_4` ist eine kompakte und wiederverwendbare Lösung für alle Anwendungen, die eine invertierte Freigabesteuerung über mehrere Kanäle benötigen. Durch die Kombination von Adapter, Inverter und Event‑Gate wird eine klare, leicht verständliche Schnittstelle geschaffen, die sich nahtlos in industrielle Steuerungssysteme integrieren lässt. Die fehlenden Zustandsübergänge und die rein kombinatorische Logik machen den Baustein besonders robust und deterministisch.