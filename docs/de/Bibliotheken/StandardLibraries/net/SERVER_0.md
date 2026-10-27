# SERVER_0

* * * * * * * * * *

## Einleitung

Der SERVER_0 Funktionsblock dient als Gegenstelle zu einem CLIENT_0 Block. Er ermöglicht die Einrichtung einer Server-Client-Verbindung ohne Austausch von Nutzdaten über Daten-Ein-/Ausgänge.

![SERVER_0](SERVER_0.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (Typ: Event) - Aktiviert den Server für eine Client-Verbindung (QI = TRUE) oder schließt den Server (QI = FALSE)
- **RSP** (Typ: Event) - Sendet eine Antwort/Bestätigung an den Client

### **Ereignis-Ausgänge**

- **INITO** (Typ: Event) - Signalisiert Bereitschaft für Client-Verbindung (QI = TRUE) oder geschlossenen Zustand (QI = FALSE)
- **IND** (Typ: Event) - Zeigt an, dass eine Anfrage vom Client eingegangen ist

### **Daten-Eingänge**

- **QI** (BOOL) - Qualifizierer für INIT-Operation
- **ID** (WSTRING) - Identifikator für die Server-Instanz

### **Daten-Ausgänge**

- **QO** (BOOL) - Qualifizierer für Statusausgabe
- **STATUS** (WSTRING) - Statusinformationen

## Funktionsweise

Der SERVER_0 Block verwaltet die Verbindung.

1. **Initialisierung**: Aktivierung über INIT.
2. **Synchronisation**:
   - Wenn der Client eine Anfrage sendet (REQ), wird am Server das IND-Ereignis ausgelöst.
   - Der Server kann daraufhin mit RSP antworten, was beim Client zum CNF-Ereignis führt.
   Es werden dabei keine Nutzdaten (SD_x, RD_x) übertragen.

## Technische Besonderheiten

- Keine Daten-Ein-/Ausgänge vorhanden
- WSTRING-Typ für ID und STATUS
- Generische Implementierung durch 'GEN_SERVER' Klassenattribut

## Zustandsübersicht

- **Nicht initialisiert**: Server inaktiv
- **Initialisiert**: Server bereit
- **Indication (IND)**: Client hat angefragt
- **Response (RSP)**: Server antwortet

## Wichtig: `IND` mit `RSP` verbinden

Jeder `SERVER_0` braucht `IND` → `RSP` direkt am eigenen Baustein. Ohne `RSP` wartet FORTE in `onServerMethodCall` bis zu 4 s und hält dabei die globale Service-Sperre von open62541. In dieser Zeit stocken **alle** OPC-UA-Publishes der Station, nicht nur die der Methode. Die Reaktion der Anwendung hängt als zweite, unabhängige Verbindung am selben `IND`; `RSP` wird nie verzögert.

- `SERVER.IND` → `SERVER.RSP`: Selbstschleife direkt am Baustein, nicht über Anwendungslogik.
- `SERVER.IND` → Reaktion der Anwendung: zweite, unabhängige Verbindung am selben `IND`.
- Bei Methoden mit Rückgabewert (z. B. `SERVER_1_2`) wird `RSP` erst nach der Berechnung ausgelöst. Die Berechnung muss deshalb sofort fertig sein und darf nicht auf ein weiteres Ereignis warten.

## Anwendungsszenarien

- Ereignis-Synchronisation
- Signalisierung von Zuständen ohne Dateninhalt

## Metadaten

| Attribut    | Wert                            |
| :---------- | :------------------------------ |
| Copyright   | (c) 2025 Monika Wenger          |
| Lizenz      | EPL-2.0                         |
| Version     | 3.0 (2025-12-05, Monika Wenger) |
| 4diac-Paket | iec61499::net                   |

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
