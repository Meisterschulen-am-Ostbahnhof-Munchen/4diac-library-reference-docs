# SPLIT_SIGNED_REAL

![SPLIT_SIGNED_REAL](./SPLIT_SIGNED_REAL.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **SPLIT_SIGNED_REAL** teilt einen vorzeichenbehafteten `REAL`-Eingangswert `Y` in zwei einseitige Betragswerte auf:

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
| `Y` | `REAL` | Vorzeichenbehafteter Eingangsmesswert |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `NEG_MAG` | `REAL` | `MAX(0, -Y)` – Betrag der negativen Auslenkung |
| `POS_MAG` | `REAL` | `MAX(0, Y)` – Betrag der positiven Auslenkung |

## Funktionsweise

Beim Eintreffen des Ereignisses `REQ` führt der Baustein folgende Aufteilungslogik aus:

```pascal
NEG_MAG := MAX(REAL#0.0, -Y);
POS_MAG := MAX(REAL#0.0, Y);
```

Der Baustein feuert bei jedem `REQ`-Ereignis die Bestätigung `CNF`. Eine Ereignis-Filterung bei gleichbleibenden Werten findet auf dieser Basisebene nicht statt (diese wird im Adapter-Wrapper **AR_SPLIT_SIGNED** über **E_D_FF_ANY** D-Flip-Flops realisiert).

## Gleitkomma-Verhalten

Da `REAL` ein Gleitkommatyp ist, tritt das Asymmetrie-Problem von Zweierkomplement-Ganzzahlen nicht auf. Die Negation `-Y` ist im gesamten Wertebereich ohne Überlauf darstellbar.

## Anwendungsszenarien

- Aufteilung von Vorzeichen-Messwerten für getrennte Visualisierungselemente (z. B. Links/Rechts-Bargraph).
- Ansteuerung von zwei unidirektionalen Aktoren (z. B. Ventil Heben vs. Ventil Senken) aus einem bipolaren Sollwert.
- Vorverarbeitung in Regelkreisen mit richtungsabhängigen Totbändern oder Kennlinien.

## Vergleich mit ähnlichen Bausteinen

- **SPLIT_SIGNED_REAL**: Reine Value/Event-Berechnung für `REAL` (Basic FB).
- **AR_SPLIT_SIGNED**: Composite-FB Wrapper mit Adapter-Schnittstellen (`adapter::types::unidirectional::AR`) und automatischer Ereignis-Filterung per D-Flip-Flop (`E_D_FF_ANY`) je Ausgangsseite.

## Fazit

**SPLIT_SIGNED_REAL** bietet eine robuste, überlaufsichere Aufteilung von `REAL`-Werten in einseitige Betragskomponenten.
