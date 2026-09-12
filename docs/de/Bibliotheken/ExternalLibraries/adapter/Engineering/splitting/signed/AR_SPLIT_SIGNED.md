# AR_SPLIT_SIGNED

![AR_SPLIT_SIGNED](./AR_SPLIT_SIGNED.svg)

* * * * * * * * * *

## Einleitung

Der **AR_SPLIT_SIGNED** ist ein Adapter-Wrapper-Funktionsbaustein (Composite FB) zum Aufteilen eines vorzeichenbehafteten `REAL`-Messwerts in zwei einseitige Betragskomponenten über unidirektionale Adapter (`adapter::types::unidirectional::AR`):

- `NEG_MAG`: Betrag der negativen Auslenkung (`MAX(0, -Y)`)
- `POS_MAG`: Betrag der positiven Auslenkung (`MAX(0, Y)`)

Der Baustein kapselt den Berechnungs-FB **SPLIT_SIGNED_REAL** sowie zwei Entprell-Bausteine vom Typ **E_D_FF_ANY**, um Ereignisse auf den Plugs `NEG_MAG` und `POS_MAG` **nur bei tatsächlicher Wertänderung** der jeweiligen Seite auszulösen.

## Schnittstellenstruktur

### **Adapter-Sockets (Eingang)**

| Name | Typ | Kommentar |
|---|---|---|
| `Y` | `adapter::types::unidirectional::AR` | Vorzeichenbehafteter Eingangswert (AR/AX-Adapter) |

### **Adapter-Plugs (Ausgang)**

| Name | Typ | Kommentar |
|---|---|---|
| `NEG_MAG` | `adapter::types::unidirectional::AR` | Betrag der negativen Auslenkung (`MAX(0, -Y)`), Event nur bei Änderung |
| `POS_MAG` | `adapter::types::unidirectional::AR` | Betrag der positiven Auslenkung (`MAX(0, Y)`), Event nur bei Änderung |

## Funktionsweise

Intern besteht das Composite-Netzwerk aus drei Bausteinen:

1. **SPLIT (SPLIT_SIGNED_REAL)**: Berechnet `NEG_MAG` und `POS_MAG` aus `Y.D1` bei jedem `Y.E1`-Ereignis.
2. **DEDUP_NEG (E_D_FF_ANY)**: Vergleicht das neue `NEG_MAG` mit dem bisherigen Wert. Nur wenn sich der Wert geändert hat, wird ein Ereignis an `NEG_MAG.E1` ausgegeben.
3. **DEDUP_POS (E_D_FF_ANY)**: Vergleicht das neue `POS_MAG` mit dem bisherigen Wert. Nur wenn sich der Wert geändert hat, wird ein Ereignis an `POS_MAG.E1` ausgegeben.

```
Y (Adapter-Socket)
 ├──> SPLIT (SPLIT_SIGNED_REAL)
       ├──> NEG_MAG ──> DEDUP_NEG (E_D_FF_ANY) ──> NEG_MAG (Adapter-Plug)
       └──> POS_MAG ──> DEDUP_POS (E_D_FF_ANY) ──> POS_MAG (Adapter-Plug)
```

## Technische Besonderheiten

- **Adapterbasierte Architektur:** Vollständig kompatibel mit der unidirektionalen Adapterfamilie `adapter::types::unidirectional::AR`.
- **Ereigniseffizienz:** Vermeidet unnötige Event-Kaskaden, da `NEG_MAG.E1` und `POS_MAG.E1` unabhängig voneinander nur bei einer echten Datenänderung gefeuert werden.
- **Überlaufsicherheit:** Vererbt die Sättigungslogik von `SPLIT_SIGNED_REAL` für Zweierkomplement-Ganzzahlgrenzen.

## Anwendungsszenarien

- Anbindung von bipolaren Gebern (z. B. Lenkwinkelsensor, Neigungssensor, Joystick) an zwei getrennte VT-Bargraphen.
- Adapterbasierte Ansteuerung von Zylinderantrieben (Ausfahren/Einfahren).

## Vergleich mit ähnlichen Bausteinen

- **AR_SPLIT_SIGNED**: Composite FB mit Adapter-Schnittstellen und automatischer Event-Deduplizierung.
- **SPLIT_SIGNED_REAL**: Unterlagerter Basic FB für reine Signalberechnung ohne Adapter.

## Fazit

**AR_SPLIT_SIGNED** bietet eine elegante, adapterbasierte und ereigniseffiziente Lösung zur Vorzeichen-Signalaufteilung in IEC 61499 Anwendungen.
