# SPLIT_SIGNED_LINT

![SPLIT_SIGNED_LINT](./SPLIT_SIGNED_LINT.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **SPLIT_SIGNED_LINT** teilt einen vorzeichenbehafteten `LINT`-Eingangswert `Y` in zwei einseitige Betragswerte auf:

- `NEG_MAG = MAX(0, -Y)` (Betrag der negativen Auslenkung)
- `POS_MAG = MAX(0, Y)` (Betrag der positiven Auslenkung)

Er dient als primärer Berechnungsbaustein für gerichtete Anzeigen (z. B. getrennte Balkenanzeigen / Bargraphen für Links- und Rechtsauslenkung aus einer Mittelstellung) oder getrennte Ansteuerungen (z. B. für Heben/Senken oder Vorwärts/Rückwärts).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|---|---|---|
| `REQ` | `Event` | Ausführung der Aufteilung anfordern (mit `Y`) |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `CNF` | `Event` | Bestätigung der Berechnung (mit `NEG_MAG`, `POS_MAG`) |

### **Daten-Eingänge**

| Name | Typ | Kommentar |
|---|---|---|
| `Y` | `LINT` | Vorzeichenbehafteter Eingangsmesswert |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `NEG_MAG` | `LINT` | `MAX(0, -Y)` – Betrag der negativen Auslenkung |
| `POS_MAG` | `LINT` | `MAX(0, Y)` – Betrag der positiven Auslenkung |

## Funktionsweise

Beim Eintreffen des Ereignisses `REQ` führt der Baustein folgende Aufteilungslogik aus:

```pascal
IF Y = LINT#-9223372036854775808 THEN
    NEG_MAG := LINT#9223372036854775807;
ELSE
    NEG_MAG := MAX(LINT#0, -Y);
END_IF;
POS_MAG := MAX(LINT#0, Y);
```

Der Baustein feuert bei jedem `REQ`-Ereignis die Bestätigung `CNF`. Eine Ereignis-Filterung bei gleichbleibenden Werten findet auf dieser Basisebene nicht statt (diese wird im Adapter-Wrapper **ALI_SPLIT_SIGNED** über **E_D_FF_ANY** D-Flip-Flops realisiert).

## Überlaufbehandlung & Sättigung

Bei Zweierkomplement-Ganzzahltypen ist der Betrag des negativen Minimums (`-9223372036854775808`) um 1 größer als das darstellbare positive Maximum (`9223372036854775807`). Eine mathematische Negation von `LINT#-9223372036854775808` würde ohne Sonderbehandlung zu einem Ganzzahlüberlauf und damit zu einem fehlerhaften Vorzeichenwechsel führen.

Der **SPLIT_SIGNED_LINT** fängt diesen Sonderfall explizit ab:

- Wenn `Y = LINT#-9223372036854775808`, wird `NEG_MAG` auf den maximal möglichen positiven Wert `LINT#9223372036854775807` gesättigt.
- Regelungstechnischer Vorteil: Sättigung am Messbereichsende statt Vorzeichenumkehr oder Überlauf.

## Anwendungsszenarien

- Aufteilung von Vorzeichen-Messwerten für getrennte Visualisierungselemente (z. B. Links/Rechts-Bargraph).
- Ansteuerung von zwei unidirektionalen Aktoren (z. B. Ventil Heben vs. Ventil Senken) aus einem bipolaren Sollwert.
- Vorverarbeitung in Regelkreisen mit richtungsabhängigen Totbändern oder Kennlinien.

## Vergleich mit ähnlichen Bausteinen

- **SPLIT_SIGNED_LINT**: Reine Value/Event-Berechnung für `LINT` (Basic FB).
- **ALI_SPLIT_SIGNED**: Composite-FB Wrapper mit Adapter-Schnittstellen (`adapter::types::unidirectional::ALI`) und automatischer Ereignis-Filterung per D-Flip-Flop (`E_D_FF_ANY`) je Ausgangsseite.

## Fazit

**SPLIT_SIGNED_LINT** bietet eine robuste, überlaufsichere Aufteilung von `LINT`-Werten in einseitige Betragskomponenten.
