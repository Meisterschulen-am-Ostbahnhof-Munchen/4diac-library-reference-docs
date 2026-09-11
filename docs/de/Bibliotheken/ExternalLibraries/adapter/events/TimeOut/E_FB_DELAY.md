# E_FB_DELAY


![E_FB_DELAY_ecc](./E_FB_DELAY_ecc.svg)

![E_FB_DELAY](./E_FB_DELAY.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock **E_FB_DELAY** realisiert eine verzögerte Ereignisweiterleitung. Ein eingehendes Ereignis (z. B. ein Startsignal) wird nach Ablauf einer konfigurierbaren Zeitverzögerung als Ausgangsereignis ausgegeben. Der Block ähnelt funktional einem Einschaltverzögerer (TON) aus der IEC‑61131‑Welt, ist jedoch für die ereignisgesteuerte Verarbeitung in 4diac‑Anwendungen optimiert. Neben der Verzögerungsfunktion bietet er einen zyklischen Abfrage‑Mechanismus, um die abgelaufene Zeit aktuell zu halten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `REQ` | Event | Normaler Ausführungsaufruf; aktualisiert `ET`, solange der Timer aktiv ist. |
| `START` | Event | Startet die verzögerte Ereignisweitergabe. |
| `STOP` | Event | Stoppt die Verzögerung und setzt den Timer zurück. |

### **Ereignis-Ausgänge**
| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `CNF` | Event | Bestätigung für den `REQ`‑Aufruf. |
| `STARTO` | Event | Wird bei erfolgreichem Start ausgegeben. |
| `STOPO` | Event | Wird bei erfolgreichem Stopp ausgegeben. |
| `EO` | Event | Verzögertes Ereignis; wird ausgelöst, wenn die Verzögerungszeit abgelaufen ist. |

### **Daten-Eingänge**
| Variable | Typ | Kommentar |
|----------|-----|-----------|
| `DT` | TIME | Verzögerungszeit, muss größer als 0 sein. |

### **Daten-Ausgänge**
| Variable | Typ | Kommentar |
|----------|-----|-----------|
| `Q` | BOOL | Zeigt an, ob der Timer aktiv ist (`TRUE` = gestartet). |
| `PT` | TIME | Prozesszeit (hier statisch, wird nicht modifiziert). |
| `ET` | TIME | Abgelaufene Zeit seit dem letzten Start. |

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

Der Funktionsblock verwendet eine interne Zeitmessung auf Basis von `NOW_MONOTONIC()`, die eine monotone Uhr ohne Systemzeit‑Springe bereitstellt. Der Ablauf gliedert sich in drei Phasen:

1. **Start** – Durch ein `START`‑Ereignis wird die Startzeit aufgezeichnet, `Q` auf `TRUE` gesetzt und `ET` auf 0 zurückgesetzt. Das Ereignis `STARTO` wird ausgegeben.
2. **Zyklische Abfrage** – Solange `Q = TRUE`, muss das `REQ`‑Ereignis regelmäßig (z. B. von einem `E_CYCLE`‑Baustein) aufgerufen werden. Bei jedem `REQ` wird `ET` als Differenz zwischen aktueller Zeit und Startzeit berechnet. Überschreitet `ET` die eingestellte Verzögerungszeit `DT`, wird das Ereignis `EO` ausgelöst.
3. **Abschluss oder Stopp** – Beim Auslösen von `EO` werden `Q` auf `FALSE` und `ET` auf 0 gesetzt. Ein `STOP`‑Ereignis kann die Verzögerung jederzeit abbrechen; auch hier werden `Q` und `ET` zurückgesetzt und `STOPO` wird ausgegeben.

Nach einem `START` kann nur ein einziger `EO`‑Ausgang ausgelöst werden. Erst ein erneutes `START` aktiviert den Timer wieder.

## Technische Besonderheiten

- **Zeitbasis**: Verwendet `NOW_MONOTONIC()` – eine monotone Uhr, die unabhängig von Systemzeit‑Anpassungen ist.
- **Keine eigene Zeitbasis**: Der FB besitzt keinen eigenen Timer‑Interrupt, sondern muss über das `REQ`‑Ereignis zyklisch aufgerufen werden. Dadurch ist die Aktualität von `ET` abhängig von der Aufruffrequenz.
- **Verzögerungsauslösung** erfolgt über die Bedingung `[ET >= DT]` im ECC. Da nach der Auslösung von `EO` `ET` auf 0 gesetzt wird, kann ein späteres `REQ` nicht erneut ein `EO` auslösen, solange kein neuer `START` erfolgt.
- **`PT`** wird als Ausgang geführt, jedoch nie verändert – sie kann zur Dokumentation oder Weiterverarbeitung verwendet werden.

## Zustandsübersicht

Der FB besitzt die folgenden Zustände im Basic‑FB‑Modell:

| Zustand | Beschreibung |
|---------|--------------|
| `Initial_State` | Ruhezustand; wartet auf `START`, `STOP` oder `REQ`. |
| `REQ` | Verarbeitet zyklische Abfragen; aktualisiert `ET` und gibt `CNF` aus. |
| `START` | Reagiert auf `START`, setzt Startzeit, aktiviert Timer und gibt `STARTO` aus. |
| `STOP` | Reagiert auf `STOP`, deaktiviert Timer und gibt `STOPO` aus. |
| `EO` | Endzustand der Verzögerung; setzt `Q` und `ET` zurück und gibt `EO` aus. |

**Übergänge:**

- `Initial_State` → `REQ` bei `REQ`‑Ereignis
- `Initial_State` → `START` bei `START`‑Ereignis
- `Initial_State` → `STOP` bei `STOP`‑Ereignis
- `REQ` → `EO` falls `[ET >= DT]`
- `REQ` → `Initial_State` sonst (immer)
- `START` → `EO` falls `[ET >= DT]` (sofort wenn DT=0)
- `START` → `Initial_State` sonst
- `STOP` → `Initial_State` (immer)
- `EO` → `Initial_State` (immer)

## Anwendungsszenarien

- **Verzögerte Signalweiterleitung**: Ein Startsignal soll erst nach einer bestimmten Zeit an einen nachfolgenden Funktionsblock weitergegeben werden – z. B. zum Warten auf ein physikalisches System.
- **Zeitüberwachung**: Überwachung, ob ein Ereignis innerhalb einer vorgegebenen Zeitspanne eintritt; bei Überschreitung wird über `EO` ein Alarm ausgelöst.
- **Schutz‑ und Sicherheitslogik**: Zeitverzögerungen vor dem Auslösen kritischer Aktionen.
- **Test und Simulation**: Erzeugung von zeitversetzten Ereignissen für Testabläufe.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Eigenschaften |
|----------|---------------|
| **E_FB_DELAY** | Ereignisgesteuert, benötigt externe Taktung über `REQ`, `ET` wird nur bei Aufruf aktualisiert, Keine Selbsttaktung. |
| **TON (IEC 61131)** | Selbsttaktend, integrierte Zeitbasis, arbeitet kontinuierlich ohne externen Aufruf, Ausgang `Q` bleibt aktiv solange Eingang `IN` = TRUE. |
| **E_CYCLE + E_DELAY** | Kombination aus zyklischem Taktgeber und Verzögerung; separate Bausteine, aber weniger flexibel in der Ereignisbehandlung. |

Der `E_FB_DELAY` bietet den Vorteil, dass er vollständig in eine ereignisbasierte 4diac‑Architektur integriert ist und eine einfache Abfrage der abgelaufenen Zeit über `ET` erlaubt. Allerdings muss die Aufruffrequenz hoch genug sein, um eine präzise Verzögerung zu gewährleisten.

## Fazit

Der **E_FB_DELAY** ist ein kompakter und flexibler Funktionsblock zur Realisierung von Zeitverzögerungen in ereignisgesteuerten Anwendungen. Durch seine klare Schnittstelle und die Verwendung einer monotonen Uhr eignet er sich besonders für industrielle Steuerungsszenarien, in denen eine zuverlässige, zyklische Zeitmessung erforderlich ist. Die Notwendigkeit eines externen `REQ`‑Signals stellt eine Beeinträchtigung dar, die durch den Entwickler berücksichtigt werden muss, aber gleichzeitig eine präzise Kontrolle über die Aktualisierungsrate der Zeitmessung ermöglicht.