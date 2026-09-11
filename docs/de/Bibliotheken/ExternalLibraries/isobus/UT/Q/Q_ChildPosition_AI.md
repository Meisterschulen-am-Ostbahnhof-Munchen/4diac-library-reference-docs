# Q_ChildPosition_AI

![Q_ChildPosition_AI](./Q_ChildPosition_AI.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `Q_ChildPosition_AI` dient zur Steuerung der Position eines Kind-Objekts gemäß ISO 11783-6 (Teil 6, F.16). Er ist eine spezielle Variante des Bausteins `Q_ChildPosition`, bei dem die X- und Y‑Koordinaten nicht über ein direktes REQ-Datenpaar, sondern über zwei unidirektionale AI‑Adapter‑Sockets empfangen werden. Diese Architektur ermöglicht eine lose Kopplung und eine flexible Anbindung an verschiedene Datenquellen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name  | Typ   | Kommentar                                   |
|-------|-------|---------------------------------------------|
| INIT  | EInit | Service-Initialisierung                     |

Das INIT‑Ereignis initialisiert den internen Baustein und übernimmt die Parameter `u16ObjId`, `u16ObjIdParent` und `xScale`.

### **Ereignis-Ausgänge**

| Name  | Typ   | Kommentar                                           |
|-------|-------|-----------------------------------------------------|
| INITO | EInit | Initialisierungsbestätigung                        |
| CNF   | Event | Bestätigung des angeforderten Dienstes              |

Das CNF‑Ereignis wird nach jeder erfolgreichen Positionsänderung ausgelöst und liefert die Ergebnisse (STATUS, alte Positionen, Rückgabewert).

### **Daten-Eingänge**

| Name                     | Typ  | Kommentar                                                                        |
|--------------------------|------|----------------------------------------------------------------------------------|
| `u16ObjId`               | UINT | Objektidentifikator, Standardwert `ID_NULL`                                      |
| `u16ObjIdParent`         | UINT | Identifikator des Elternobjekts, Standardwert `ID_NULL`                          |
| `xScale`                 | BOOL | Skalierungsflag: `FALSE` (Standard) = unverändert durchreichen, `TRUE` = Skalierung mit dem DM/SKM‑Faktor des Elternobjekts |

### **Daten-Ausgänge**

| Name               | Typ     | Kommentar                                        |
|--------------------|---------|--------------------------------------------------|
| `STATUS`           | STRING  | Dienststatus; wird direkt vom internen `Q_ChildPosition` übernommen |
| `s16OldXposition`  | INT     | Vorherige X‑Position (Passthrough)               |
| `s16OldYposition`  | INT     | Vorherige Y‑Position (Passthrough)               |
| `s16result`        | INT     | Rückgabewert des Dienstes (Passthrough)          |

### **Adapter**

| Name          | Typ                                | Kommentar                                                       |
|---------------|------------------------------------|-----------------------------------------------------------------|
| `s16Xposition` | `adapter::types::unidirectional::AI` | Neue X‑Position relativ zur oberen linken Ecke des Elternobjekts |
| `s16Yposition` | `adapter::types::unidirectional::AI` | Neue Y‑Position relativ zur oberen linken Ecke des Elternobjekts |

## Funktionsweise

Der Baustein wird über das `INIT`‑Ereignis gestartet, wobei die Objekt‑IDs und das Skalierungsflag an den internen `Q_ChildPosition` übergeben werden. Danach wartet er auf Ereignisse an den beiden AI‑Adapter‑Sockets. Sobald eines der beiden Sockets ein Ereignis (E1) liefert, liest der FB den aktuellen Datenwert (D1) dieses Sockets sowie den zuletzt empfangenen Wert des anderen Sockets und führt eine Positionsänderung durch. Dazu wird das `REQ`‑Ereignis des internen `Q_ChildPosition` ausgelöst, das die gelesenen X‑ und Y‑Werte als neue Position verarbeitet.

Das Ergebnis (alter und neuer Zustand) wird über das `CNF`‑Ereignis nach außen zurückgemeldet. Die beiden AI‑Sockets sind unidirektional; sie liefern nur Daten an den FB, ohne dass eine Rückantwort über den Adapter erfolgt.

## Technische Besonderheiten

- Der FB ist ein reiner Wrapper um `Q_ChildPosition` und verwendet dessen Implementierung unverändert.
- Die Positionseingänge sind als AI‑Adapter‑Sockets vom Typ `unidirectional` ausgeführt, wodurch eine klare Trennung zwischen Ereignis‑ und Datenschnittstelle entsteht.
- Das optionale Skalierungsflag `xScale` erlaubt eine Anpassung der Positionswerte an die Skalierung des Elternobjekts, ohne zusätzliche externe Logik.
- Die beiden Adapter können unabhängig voneinander und in beliebiger Reihenfolge Ereignisse liefern; der FB verarbeitet jedes Ereignis und verwendet dabei den jeweils aktuellen Wert auf beiden Sockets.

## Zustandsübersicht

Der Baustein besitzt keinen expliziten internen Zustandsautomaten, sondern arbeitet ereignisgesteuert:

- **Initialisierungszustand:** Nach dem `INIT`‑Ereignis wird der interne FB initialisiert, danach ist er bereit für Positionsänderungen.
- **Wartezustand:** Der FB wartet auf ein Ereignis an einem der beiden AI‑Sockets.
- **Verarbeitungszustand:** Bei Eingang eines Ereignisses wird die Positionsänderung durchgeführt und das Ergebnis über `CNF` ausgegeben.

## Anwendungsszenarien

Dieser FB eignet sich für alle Systeme, die Positionsdaten eines Kind‑Objekts (z. B. grafische Symbole) über unidirektionale Adapterschnittstellen erhalten und an ein ISOBUS‑Terminal nach ISO 11783 senden möchten. Typische Einsatzbereiche sind landwirtschaftliche Maschinen und Anbaugeräte, bei denen eine flexible Signalverkabelung und eine modulare Softwarearchitektur gefordert sind.

## Vergleich mit ähnlichen Bausteinen

Der direkte Baustein `Q_ChildPosition` erwartet die Positionsdaten über ein separates REQ‑Ereignis und direkte Eingangsvariablen. `Q_ChildPosition_AI` bietet dieselbe Funktionalität, ersetzt jedoch diese Schnittstelle durch zwei AI‑Adapter‑Sockets, wodurch die Integration in adapterbasierte Systeme deutlich erleichtert wird. Dies reduziert die Verdrahtung und erhöht die Wiederverwendbarkeit in übergeordneten Netzwerken.

## Fazit

`Q_ChildPosition_AI` ist eine sinnvolle Erweiterung für Anwendungen, die eine lose, ereignisorientierte Datenanbindung benötigen. Die Verwendung unidirektionaler AI‑Sockets macht den FB flexibel und leicht in bestehende ISOBUS‑Architekturen integrierbar, ohne auf die bewährte Funktionalität des zugrunde liegenden `Q_ChildPosition` zu verzichten. Die optionale Skalierung und die klare Schnittstellenstruktur erhöhen die Einsatzmöglichkeiten in modernen Steuerungssystemen.
