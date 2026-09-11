# AX_E_PERMIT_INVERT_1


![AX_E_PERMIT_INVERT_1_network](./AX_E_PERMIT_INVERT_1_network.svg)

![AX_E_PERMIT_INVERT_1](./AX_E_PERMIT_INVERT_1.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AX_E_PERMIT_INVERT_1` realisiert eine invertierte Event-Freigabe auf Basis von Adapter-Signalen. Er kombiniert die Funktionalität eines invertierenden Adapter-Operators (`AX_NOT_INIT`) mit einem ereignisbasierten Freigabe-Gate (`AX_E_PERMIT_1`). Dadurch wird ein Ereignis nur dann vom Eingang `EI` zum Ausgang `EO` durchgereicht, wenn das über den Adapter anliegende Freigabesignal `PERMIT` den logischen Zustand `FALSE` besitzt. Der Baustein ist als Subapplikation (SubApp) in der Bibliothek `MyLib::sys` verfügbar.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `EI`  | Event | Ereigniseingang – wird durchgelassen, wenn das Freigabesignal `FALSE` ist. |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `EO`  | Event | Ereignisausgang – feuert, wenn `EI` eintrifft und das Freigabesignal `FALSE` ist. |

### **Daten-Eingänge**

Es existieren keine Daten-Eingänge.

### **Daten-Ausgänge**

Es existieren keine Daten-Ausgänge.

### **Adapter**

| Name | Richtung | Typ | Kommentar |
|------|----------|-----|-----------|
| `PERMIT` | Socket | `adapter::types::unidirectional::AX` | Invertiertes Freigabesignal; der eingehende Adapter-Wert wird intern negiert. |

## Funktionsweise

Der Baustein verarbeitet ein Ereignis, das am Eingang `EI` anliegt, abhängig vom aktuellen Wert des Adapter-Signals `PERMIT`. Die interne Verschaltung besteht aus zwei Funktionsblöcken:

1. **`AX_NOT_INIT`** – Dieser Block erhält das Adapter-Signal `PERMIT` und erzeugt an seinem Ausgang die logische Negation des empfangenen Wertes.
2. **`AX_E_PERMIT_1`** – Dieser Block stellt ein ereignisbasiertes Freigabe-Gate dar. Er besitzt einen Ereigniseingang `EI1`, einen Ereignisausgang `EO1` und einen Adapter-Eingang `PERMIT`. Ein Ereignis wird nur dann vom Eingang zum Ausgang durchgereicht, wenn das am Adapter-Eingang anliegende Signal den Zustand `TRUE` besitzt.

Die Verbindungen innerhalb der Subapp sind:

- Ereignis `EI` → `AX_E_PERMIT_1.EI1`
- Ereignis `AX_E_PERMIT_1.EO1` → `EO`
- Adapter `PERMIT` → `AX_NOT_INIT.IN`
- Adapter `AX_NOT_INIT.OUT` → `AX_E_PERMIT_1.PERMIT`

Damit ergibt sich folgende logische Kette:

- **Wenn `PERMIT` = `TRUE`:**  
  `AX_NOT_INIT` invertiert zu `FALSE`. Das Freigabe-Gate `AX_E_PERMIT_1` ist gesperrt, und das Ereignis `EI` wird nicht nach `EO` durchgelassen.

- **Wenn `PERMIT` = `FALSE`:**  
  `AX_NOT_INIT` invertiert zu `TRUE`. Das Freigabe-Gate ist geöffnet, und ein am Eingang `EI` eintreffendes Ereignis wird unverzögert an den Ausgang `EO` weitergegeben.

## Technische Besonderheiten

- Der Baustein nutzt Adapter-Signale (Typ `AX`) zur Steuerung, nicht klassische Daten-Ein-/Ausgänge. Dadurch ist eine enge Kopplung an das Adapter-Protokoll `AX` gegeben.
- Die Invertierung wird durch den separaten Funktionsblock `AX_NOT_INIT` realisiert, der als eigenständige wiederverwendbare Komponente in der Bibliothek vorhanden ist.
- Es werden keine Zustände gespeichert; die Verarbeitung erfolgt rein ereignisgesteuert und kombinatorisch.
- Die Subapp ist als einfache Kombination zweier Standard-FBs aufgebaut und ermöglicht eine hohe Wartbarkeit.

## Zustandsübersicht

Da der Baustein keinen internen Speicher besitzt, lassen sich lediglich zwei externe logische Zustände unterscheiden:

| Zustand | Bedingung (`PERMIT`) | Verhalten am Ausgang `EO` |
|---------|----------------------|----------------------------|
| **Freigegeben** | `FALSE` | Eingang `EI` wird zum Ausgang `EO` durchgereicht. |
| **Gesperrt** | `TRUE` | Eingang `EI` wird ignoriert; `EO` bleibt inaktiv. |

## Anwendungsszenarien

- **Sicherheitslogik:** Freigabe von Ereignissen nur bei inaktivem/permit-negativem Signal, z. B. in Schutzeinrichtungen oder Not-Aus-Ketten.
- **Signalsteuerung:** Deaktivierung von Ereignisströmen in Automatisierungssystemen, wenn ein bestimmter Adapter-Zustand aktiv ist.
- **Protokollanpassung:** Einfache Umkehrung einer Freigabe-Logik in industriellen Steuerungen, ohne zusätzliche Programmlogik in der aufrufenden Applikation.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einem normalen Event-Freigabe-Gate (`AX_E_PERMIT_1`), das Ereignisse bei `TRUE` durchlässt, ist `AX_E_PERMIT_INVERT_1` genau invers: Es lässt Ereignisse bei `FALSE` durch. Vorteile gegenüber einer externen Invertierung des Signals sind die kompakte Kapselung und die klare Lesbarkeit. Ähnliche Bausteine könnten mit einem negierten Adapter-Signal oder einer anderen Grundlogik aufgebaut sein, aber dieser Baustein bietet eine vordefinierte, wiederverwendbare Lösung.

## Fazit

`AX_E_PERMIT_INVERT_1` ist ein nützlicher Baustein, um ereignisbasierte Freigaben in invertierter Form zu realisieren. Er kombiniert die bewährten Funktionen `AX_NOT_INIT` und `AX_E_PERMIT_1` zu einer kompakten Einheit und erleichtert damit die Implementierung von „Freigabe bei inaktivem Signal“-Logiken. Die klare Schnittstelle und die rein kombinatorische Funktionsweise machen ihn zu einer zuverlässigen Komponente in STEP- und IEC-61499-Anwendungen.
