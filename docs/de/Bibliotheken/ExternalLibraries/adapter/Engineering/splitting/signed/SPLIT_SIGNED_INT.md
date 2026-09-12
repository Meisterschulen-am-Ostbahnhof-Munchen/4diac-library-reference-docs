# SPLIT_SIGNED_INT

![SPLIT_SIGNED_INT](./SPLIT_SIGNED_INT.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **SPLIT_SIGNED_INT** teilt einen vorzeichenbehafteten `INT`-Eingangswert `Y` in zwei einseitige Betragswerte auf:

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
| `Y` | `INT` | Vorzeichenbehafteter Eingangsmewert |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `NEG_MAG` | `INT` | `MAX(0, -Y)` – Betrag der negativen Auslenkung |
| `POS_MAG` | `INT` | `MAX(0, Y)` – Betrag der positiven Auslenkung |

## Funktionsweise

Beim Eintreffen des Ereignisses `REQ` führt der Baustein folgende Aufteilungslogik aus:

```pascal
POS_MAG := MAX(INT#0, Y);
IF Y = INT#-32768 THEN
    NEG_MAG := INT#32767;
ELSE
    NEG_MAG := MAX(INT#0, -Y);
END_IF;
```

Der Baustein feuert bei jedem `REQ`-Ereignis die Bestätigung `CNF`. Eine Ereignis-Deduplizierung bei gleichbleibenden Werten findet auf dieser Basisebene nicht statt (diese wird im Adapter-Wrapper **AI_SPLIT_SIGNED** über `E_D_FF_ANY` realisiert).

## Überlaufbehandlung & Sättigung

Bei Zweierkomplement-Ganzzahltypen ist der Betrag des negativen Minimums (`-32768`) um 1 größer als das darstellbare positive Maximum (`32767`). Eine mathematische Negation von `INT#-32768` würde ohne Sonderbehandlung zu einem Ganzzahlüberlauf und damit zu einem fehlerhaften Vorzeichenwechsel führen.

Der **SPLIT_SIGNED_INT** fängt diesen Sonderfall explizit ab:

- Wenn `Y = INT#-32768`, wird `NEG_MAG` auf den maximal möglichen positiven Wert `INT#32767` gesättigt.
- Regelungstechnischer Vorteil: Sättigung am Messbereichsende statt Vorzeichenumkehr oder Überlauf.

## Anwendungsszenarien

- Aufteilung von Vorzeichen-Messwerten für getrennte Visualisierungselemente (z. B. Links/Rechts-Bargraph).
- Ansteuerung von zwei unidirektionalen Aktoren (z. B. Ventil Heben vs. Ventil Senken) aus einem bipolaren Sollwert.
- Vorverarbeitung in Regelkreisen mit richtungsabhängigen Totbändern oder Kennlinien.

## Vergleich mit ähnlichen Bausteinen

- **SPLIT_SIGNED_INT**: Reine Value/Event-Berechnung für `INT` (Basic FB).
- **AI_SPLIT_SIGNED**: Composite-FB Wrapper mit Adapter-Schnittstellen (`adapter::types::unidirectional::AI`) und automatischer Ereignis-Deduplizierung je Ausgangsseite.

## Fazit

**SPLIT_SIGNED_INT** bietet eine robuste, überlaufsichere Aufteilung von `INT`-Werten in einseitige Betragskomponenten.
