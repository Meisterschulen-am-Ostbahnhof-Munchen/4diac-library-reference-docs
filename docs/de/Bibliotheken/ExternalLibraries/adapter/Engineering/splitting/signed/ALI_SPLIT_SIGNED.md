# ALI_SPLIT_SIGNED

![ALI_SPLIT_SIGNED](./ALI_SPLIT_SIGNED.svg)

* * * * * * * * * *

## Einleitung

Der **ALI_SPLIT_SIGNED** ist ein Adapter-Wrapper-Funktionsbaustein (Composite FB) zum Aufteilen eines vorzeichenbehafteten `LINT`-Messwerts in zwei einseitige Betragskomponenten über unidirektionale Adapter (`adapter::types::unidirectional::ALI`):

- `NEG_MAG`: Betrag der negativen Auslenkung (`MAX(0, -Y)`)
- `POS_MAG`: Betrag der positiven Auslenkung (`MAX(0, Y)`)

Der Baustein kapselt den Berechnungs-FB **SPLIT_SIGNED_LINT** sowie zwei D-Flip-Flop-Bausteine vom Typ **E_D_FF_ANY**, um Ereignisse auf den Plugs `NEG_MAG` und `POS_MAG` **nur dann auszulösen, wenn sich der neue Eingangswert vom bisherigen Ausgangswert unterscheidet** (`D <> Q`).

## Schnittstellenstruktur

### **Adapter-Sockets (Eingang)**

| Name | Typ | Kommentar |
|---|---|---|
| `Y` | `adapter::types::unidirectional::ALI` | Vorzeichenbehafteter Eingangswert (ALI-Adapter-Socket) |

### **Adapter-Plugs (Ausgang)**

| Name | Typ | Kommentar |
|---|---|---|
| `NEG_MAG` | `adapter::types::unidirectional::ALI` | Betrag der negativen Auslenkung (`MAX(0, -Y)`), Ereignis nur bei Wertänderung |
| `POS_MAG` | `adapter::types::unidirectional::ALI` | Betrag der positiven Auslenkung (`MAX(0, Y)`), Ereignis nur bei Wertänderung |

## Funktionsweise

Intern besteht das Composite-Netzwerk aus drei Bausteinen:

1. **SPLIT (SPLIT_SIGNED_LINT)**: Berechnet `NEG_MAG` und `POS_MAG` aus `Y.D1` bei jedem `Y.E1`-Ereignis.
2. **DEDUP_NEG (E_D_FF_ANY)**: Ein ereignisgesteuertes D-Flip-Flop, das das Eingangsereignis `CLK` nur dann an den Ereignisausgang `EO` weiterleitet und `Q := D` aktualisiert, wenn der neue Wert `D` ungleich dem bisherigen Ausgangswert `Q` ist (`D <> Q`).
3. **DEDUP_POS (E_D_FF_ANY)**: Ein zweites D-Flip-Flop derselben Bauart, das das Ereignis `POS_MAG.E1` ebenfalls nur bei einer echten Datenänderung (`POS_MAG.D1 <> POS_MAG.Q`) ausgibt.

```
Y (ALI-Adapter-Socket)
 ├──> SPLIT (SPLIT_SIGNED_LINT)
       ├──> NEG_MAG ──> DEDUP_NEG (E_D_FF_ANY D-Flip-Flop) ──> NEG_MAG (ALI-Adapter-Plug)
       └──> POS_MAG ──> DEDUP_POS (E_D_FF_ANY D-Flip-Flop) ──> POS_MAG (ALI-Adapter-Plug)
```

## Technische Besonderheiten

- **Adapterbasierte Architektur:** Vollständig kompatibel mit der unidirektionalen Adapterfamilie `adapter::types::unidirectional::ALI`.
- **Ereignis-Filterung per D-Flip-Flop:** Durch die `E_D_FF_ANY` Bausteine wird sichergestellt, dass an `NEG_MAG.E1` bzw. `POS_MAG.E1` per Definition nur dann ein Ausgangsereignis erzeugt wird, wenn sich der neue Wert `D` vom bisherigen Zustand `Q` unterscheidet.
- **Überlaufsicherheit:** Vererbt die Sättigungslogik von `SPLIT_SIGNED_LINT` für Zweierkomplement-Ganzzahlgrenzen.

## Anwendungsszenarien

- Anbindung von bipolaren Gebern (z. B. Lenkwinkelsensor, Neigungssensor, Joystick) an zwei getrennte VT-Bargraphen.
- Adapterbasierte Ansteuerung von Zylinderantrieben (Ausfahren/Einfahren).

## Vergleich mit ähnlichen Bausteinen

- **ALI_SPLIT_SIGNED**: Composite FB mit Adapter-Schnittstellen und D-Flip-Flop Event-Filterung.
- **SPLIT_SIGNED_LINT**: Unterlagerter Basic FB für reine Signalberechnung ohne Adapter.

## Fazit

**ALI_SPLIT_SIGNED** bietet eine elegante, adapterbasierte und durch D-Flip-Flops ereigniseffiziente Lösung zur Vorzeichen-Signalaufteilung in IEC 61499 Anwendungen.
