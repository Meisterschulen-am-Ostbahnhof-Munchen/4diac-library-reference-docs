# PositionMarkerFSA

![PositionMarkerFSA](./PositionMarkerFSA.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **PositionMarkerFSA** ist ein Adapter-Wrapper um den Baustein `PositionMarkerFS`. Er ermöglicht die Übergabe eines Sollwertes (REAL) über einen AR-Adapter-Socket, anstatt über ein separates REQ-Ereignis und einen rValue-Eingang. Dadurch wird die Anbindung an ISOBUS- oder CAN‑basierte Datenströme vereinfacht, bei denen physikalische Werte bereits als Adapterwerte vorliegen. Über- und Unterschreitung der zulässigen Positionsgrenzen werden über AX‑Adapter‑Plugs ausgegeben.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ     | Mit Variablen | Kommentar |
|----------|---------|---------------|-----------|
| `INIT`   | `EInit` | `stObj`, `xScale` | Initialisiert den internen Baustein mit den Markerobjekt-Eigenschaften und der Skalierungsoption. |

### **Ereignis-Ausgänge**

| Ereignis | Typ     | Mit Variablen      | Kommentar |
|----------|---------|--------------------|-----------|
| `INITO`  | `EInit` | –                  | Bestätigt die erfolgreiche Initialisierung. |
| `CNF`    | `Event` | `STATUS`, `s16result` | Wird nach einer Verarbeitung des Sollwertes ausgegeben und liefert Status- und Ergebnisdaten. |

### **Daten-Eingänge**

| Name    | Typ                | Kommentar |
|---------|--------------------|-----------|
| `stObj` | `isobus::utils::childposition::PositionMarker_S` | Definiert die Markerobjekt-Pooleigenschaften (Kind-/Eltern-IDs, Verfahrgrenzen, Mittenversatz). Wird bei INIT gesnappt. |
| `xScale`| `BOOL` | Skalierungsflag. `FALSE` (Standard) = Umgehung der Skalierung, `TRUE` = Skalierung durch den DM/SKM‑Faktor. Wird direkt an den internen Baustein durchgereicht. |

### **Daten-Ausgänge**

| Name        | Typ      | Kommentar |
|-------------|----------|-----------|
| `STATUS`    | `STRING` | Dienststatus – direkte Durchreichung vom internen `PositionMarkerFS`. |
| `s16result` | `INT`    | Rückgabewert – direkte Durchreichung vom internen `PositionMarkerFS`. |

### **Adapter**

| Typ | Richtung | Name       | Kommentar |
|-----|----------|------------|-----------|
| Socket | Eingang | `rPhys` | AR‑Adapter, der einen physikalischen REAL‑Wert als Sollwert liefert. |
| Plug   | Ausgang  | `xOver`  | AX‑Adapter, aktiv wenn der Sollwert (nach Addition von `stObj.r32Center`) den maximalen Verfahrbereich überschreitet und geklemmt wurde. |
| Plug   | Ausgang  | `xUnder` | AX‑Adapter, aktiv wenn der Sollwert (nach Addition von `stObj.r32Center`) den minimalen Verfahrbereich unterschreitet und geklemmt wurde. |

## Funktionsweise

Der Baustein delegiert seine gesamte Funktionalität an eine interne Instanz von `PositionMarkerFS`. Beim Eintreffen des `INIT`‑Ereignisses werden die Eingabedaten `stObj` und `xScale` an die interne Instanz übertragen und deren Initialisierung ausgelöst. Nach erfolgreicher Initialisierung quittiert der Baustein mit `INITO`.

Der eigentliche Sollwert wird über den AR‑Adapter `rPhys` empfangen. Sobald der Adapter ein Ereignis `E1` liefert (typischerweise bei einer Wertänderung), wird dieses direkt als `REQ`‑Ereignis an die interne Instanz weitergeleitet. Gleichzeitig wird der vom Adapter bereitgestellte Datenwert `D1` (der physikalische Sollwert) dem Eingang `rValue` des internen Bausteins zugeführt. Dadurch entfällt die Notwendigkeit, einen separaten `REQ`‑Trigger mit einem Datenwert zu koppeln.

Die interne Instanz berechnet die Position unter Berücksichtigung des Mittelpunktsversatzes und der Verfahrgrenzen. Das Ergebnis wird über `STATUS` und `s16result` ausgegeben, nachdem das Ereignis `CNF` der internen Instanz empfangen wurde. Zusätzlich werden die Ereignisse für Über‑ und Unterschreitung (falls zutreffend) an die AX‑Adapter `xOver` bzw. `xUnder` weitergegeben – jeweils ausgelöst durch das `CNF`‑Ereignis der internen Instanz.

## Technische Besonderheiten

- **Adapter‑basiertes Design:** Der Sollwert wird über einen unidirektionalen AR‑Adapter zugeführt, was die Integration in bestehende ISOBUS‑ oder CAN‑Datenströme erleichtert.
- **Transparente Durchreichung des Skalierungsflags:** `xScale` wird unverändert an den internen Baustein weitergegeben und steuert dort die optionale Skalierung durch den DM/SKM‑Faktor.
- **Rückmeldung über AX‑Adapter:** Über‑ und Unterschreitung werden als separate unidirektionale Adapter-Plugs bereitgestellt, die Ereignisse und Daten getrennt führen.
- **Standardkonformität:** Der Baustein basiert auf dem Standard ISO 11783‑6 und verwendet die in `isobus::utils` definierte Struktur `PositionMarker_S`.
- **Wrapper‑Architektur:** Die interne Instanz von `PositionMarkerFS` bleibt vollständig gekapselt, sodass alle Schnittstellen über den Adapter‑Rahmen laufen.

## Zustandsübersicht

Der Baustein besitzt keine eigene, explizit dokumentierte Zustandsmaschine. Ablaufsteuerung ergibt sich aus den Ereignissen:

- **Vor INIT:** Baustein wartet auf Initialisierung. Es werden keine Verarbeitungen durchgeführt.
- **Nach INIT:** Die interne Instanz ist initialisiert. Der Baustein kann Sollwerte vom AR‑Adapter verarbeiten und über `CNF` bestätigen.
- **Nach jeder Verarbeitung:** Die Ereignisse `CNF` (und ggf. `xOver.E1`/`xUnder.E1`) werden ausgegeben. Danach ist der Baustein bereit für den nächsten Adapterwert.

## Anwendungsszenarien

- **Landwirtschaftliche Maschinensteuerung:** Einsatz in Traktoren oder Anbaugeräten, bei denen Positionsmarker gesetzt werden müssen und der Sollwert als physikalische Größe (z. B. über ein ISOBUS‑Terminal) ankommt.
- **Automatische Spurführung:** Übernahme von Sollpositionen aus einem übergeordneten System, wobei eine direkte Kopplung an einen AR‑Adapter gewünscht ist.
- **Flexible Wertübergabe:** Überall dort, wo bisher `PositionMarkerFS` mit separatem REQ/rValue verwendet wurde, aber eine Adapter‑Schnittstelle die Netzwerk‑Topologie vereinfacht.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zum ursprünglichen `PositionMarkerFS`, der die Sollwertübergabe über ein separates `REQ`‑Ereignis und den Datenwert `rValue` vornimmt, verwendet `PositionMarkerFSA` einen AR‑Adapter–Socket. Dieser bündelt Ereignis (`E1`) und Datenwert (`D1`) in einer einzigen Schnittstelle. Der Funktionsblock `Q_NumericValue_PHYSA` verfolgt ein ähnliches Konzept für numerische Werte; `PositionMarkerFSA` ist speziell auf die Positionsmarker‑Logik zugeschnitten und bietet zusätzlich die AX‑Adapter für Überschreitungsmeldungen.

## Fazit

`PositionMarkerFSA` stellt eine elegante, adapter‑basierte Alternative zu `PositionMarkerFS` dar. Er vereinfacht die Anbindung an bestehende Datenströme und bietet eine klare, unidirektionale Kommunikation für Sollwerte und Grenzwertmeldungen. Die vollständige Kapselung der internen Logik macht den Baustein wartbar und konsistent mit anderen ISO 11783‑Bausteinen.
