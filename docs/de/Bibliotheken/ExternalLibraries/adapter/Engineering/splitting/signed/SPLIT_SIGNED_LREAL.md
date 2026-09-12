# SPLIT_SIGNED_LREAL

![SPLIT_SIGNED_LREAL](./SPLIT_SIGNED_LREAL.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **SPLIT_SIGNED_LREAL** teilt einen vorzeichenbehafteten `LREAL`-Eingangswert `Y` in zwei einseitige Betragswerte auf:

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
| `Y` | `LREAL` | Vorzeichenbehafteter Eingangsmesswert |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `NEG_MAG` | `LREAL` | `MAX(0, -Y)` – Betrag der negativen Auslenkung |
| `POS_MAG` | `LREAL` | `MAX(0, Y)` – Betrag der positiven Auslenkung |

## Funktionsweise

Beim Eintreffen des Ereignisses `REQ` führt der Baustein folgende Aufteilungslogik aus:

```pascal
NEG_MAG := MAX(LREAL#0.0, -Y);
POS_MAG := MAX(LREAL#0.0, Y);
```

Der Baustein feuert bei jedem `REQ`-Ereignis die Bestätigung `CNF`. Eine Ereignis-Filterung bei gleichbleibenden Werten findet auf dieser Basisebene nicht statt (diese wird im Adapter-Wrapper **ALR_SPLIT_SIGNED** über **E_D_FF_ANY** D-Flip-Flops realisiert).

## Gleitkomma-Verhalten

Da `LREAL` ein Gleitkommatyp ist, tritt das Asymmetrie-Problem von Zweierkomplement-Ganzzahlen nicht auf. Die Negation `-Y` ist im gesamten Wertebereich ohne Überlauf darstellbar.

## Anwendungsszenarien

- Aufteilung von Vorzeichen-Messwerten für getrennte Visualisierungselemente (z. B. Links/Rechts-Bargraph).
- Ansteuerung von zwei unidirektionalen Aktoren (z. B. Ventil Heben vs. Ventil Senken) aus einem bipolaren Sollwert.
- Vorverarbeitung in Regelkreisen mit richtungsabhängigen Totbändern oder Kennlinien.

## Vergleich mit ähnlichen Bausteinen

- **SPLIT_SIGNED_LREAL**: Reine Value/Event-Berechnung für `LREAL` (Basic FB).
- **ALR_SPLIT_SIGNED**: Composite-FB Wrapper mit Adapter-Schnittstellen (`adapter::types::unidirectional::ALR`) und automatischer Ereignis-Filterung per D-Flip-Flop (`E_D_FF_ANY`) je Ausgangsseite.

## Fazit

**SPLIT_SIGNED_LREAL** bietet eine robuste, überlaufsichere Aufteilung von `LREAL`-Werten in einseitige Betragskomponenten.
