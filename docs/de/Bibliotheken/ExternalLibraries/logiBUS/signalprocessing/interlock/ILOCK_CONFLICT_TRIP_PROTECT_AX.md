# ILOCK_CONFLICT_TRIP_PROTECT_AX

![ILOCK_CONFLICT_TRIP_PROTECT_AX](./ILOCK_CONFLICT_TRIP_PROTECT_AX.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `ILOCK_CONFLICT_TRIP_PROTECT_AX` ist die Adapter-Version von `ILOCK_CONFLICT_TRIP_PROTECT`: Er kombiniert die Trip-bei-Konflikt-Logik von `ILOCK_CONFLICT_TRIP_AX` mit der Schutz-Totzeit von `ILOCK_BLOCK_PROTECT_AX`. Der zuerst aktive Eingang wird priorisiert; eine gleichzeitige Aktivierung beider Richtungen löst sofort einen Trip aus, der nur über `EI_RESET` zurückgesetzt werden kann. Nach Freigabe des aktiven Eingangs wartet der Baustein zusätzlich die konfigurierbare Zeit `DT_PROTECT` ab, bevor er die Eingänge neu bewertet. Die gesamte Kommunikation läuft über Adapter vom Typ `unidirectional::AX`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name       | Mitgeführte Daten | Beschreibung                                                                 |
| ---------- | ------------------- | ------------------------------------------------------------------------------ |
| `EI_RESET` | –                   | Rücksetzen des Trip-Zustands; nur wirksam, wenn `UP_IN.D1` und `DOWN_IN.D1` beide FALSE sind. |
| `UPDATE`   | `DT_PROTECT`        | Aktualisiert die Schutzzeit `DT_PROTECT` zur Laufzeit, ohne den aktuellen Zustand zu verlassen. |

### **Ereignis-Ausgänge**

Keine direkten Ereignis-Ausgänge. Zustandsänderungen werden über die Ereignisse der Ausgangsadapter (Plugs) signalisiert:

- `UP_OUT.E1`, `DOWN_OUT.E1`, `TRIP_OUT.E1`

### **Daten-Eingänge**

Keine direkten Dateneingänge. Werden über die Socket-Adapter bereitgestellt:

- `UP_IN.D1` (BOOL) – Aktivierung der Aufwärts-Richtung.
- `DOWN_IN.D1` (BOOL) – Aktivierung der Abwärts-Richtung.
- `DT_PROTECT` (TIME, Initialwert `T#50ms`) – Schutz-Totzeit nach Freigabe des aktiven Eingangs.

### **Daten-Ausgänge**

Keine direkten Datenausgänge. Werden über die Plug-Adapter bereitgestellt:

- `UP_OUT.D1` (BOOL) – Signal für Aufwärts-Richtung.
- `DOWN_OUT.D1` (BOOL) – Signal für Abwärts-Richtung.
- `TRIP_OUT.D1` (BOOL) – Signal für den Trip-Zustand.

### **Adapter**

**Sockets (Eingänge)**

| Adapter   | Typ                                  | Beschreibung                            |
| --------- | ------------------------------------- | ----------------------------------------- |
| `UP_IN`   | `adapter::types::unidirectional::AX` | Eingang für die Aufwärts-Richtung.       |
| `DOWN_IN` | `adapter::types::unidirectional::AX` | Eingang für die Abwärts-Richtung.        |

**Plugs (Ausgänge)**

| Adapter    | Typ                                  | Beschreibung                    |
| ---------- | ------------------------------------- | ---------------------------------- |
| `UP_OUT`   | `adapter::types::unidirectional::AX` | Ausgang für die Aufwärts-Richtung. |
| `DOWN_OUT` | `adapter::types::unidirectional::AX` | Ausgang für die Abwärts-Richtung.  |
| `TRIP_OUT` | `adapter::types::unidirectional::AX` | Trip-Zustandsausgang.             |
| `timeOut`  | `iec61499::events::ATimeOut`         | Timer-Adapter für die Schutzzeit; der Baustein setzt `timeOut.DT` und startet ihn über `timeOut.START`. |

## Funktionsweise

Der Automat entspricht strukturell `ILOCK_CONFLICT_TRIP_PROTECT`, nur dass alle Signale über Adapter geführt werden:

1. **STOP** – Ruhezustand. Bei `UP_IN.E1[UP_IN.D1 AND NOT DOWN_IN.D1]` → **UP**; bei `DOWN_IN.E1[DOWN_IN.D1 AND NOT UP_IN.D1]` → **DOWN**; sind bei einem der beiden Ereignisse beide Datensignale TRUE → sofort **TRIP**.
2. **UP** – `UP_OUT.D1 = TRUE`. Bei `UP_IN.E1[NOT UP_IN.D1]` → **UP_STOP**; bei `DOWN_IN.E1[DOWN_IN.D1]` (Konflikt) → sofort **TRIP**.
3. **DOWN** – `DOWN_OUT.D1 = TRUE`. Bei `DOWN_IN.E1[NOT DOWN_IN.D1]` → **DOWN_STOP**; bei `UP_IN.E1[UP_IN.D1]` (Konflikt) → sofort **TRIP**.
4. **UP_STOP** / **DOWN_STOP** – Der Algorithmus `STOP` setzt alle Adapterausgänge auf FALSE, überträgt `DT_PROTECT` an `timeOut.DT` und startet den Timer. Bei `timeOut.TimeOut` → **EVAL**.
5. **EVAL** – Kein eigener Algorithmus. Neubewertung: nur `UP_IN.D1` TRUE → **UP**; nur `DOWN_IN.D1` TRUE → **DOWN**; beide FALSE → **STOP**; beide TRUE → **TRIP**.
6. **TRIP** – `TRIP_OUT.D1 = TRUE`. Verlassen nur über `EI_RESET[NOT UP_IN.D1 AND NOT DOWN_IN.D1]` → **STOP**.

Zusätzlich existiert in den Zuständen `STOP`, `UP` und `DOWN` je eine Selbstschleifen-Transition auf das Ereignis `UPDATE`, die den jeweiligen Zustand nicht verlässt und ausschließlich der Übernahme eines neuen `DT_PROTECT`-Werts dient.

## Technische Besonderheiten

- **Reine Adapter-Schnittstelle:** Alle Prozessdaten werden über `unidirectional::AX`-Adapter geführt, es gibt keine klassischen Event-/Datenports außer `EI_RESET`, `UPDATE` und `DT_PROTECT`.
- **Dynamische Totzeit:** Das Ereignis `UPDATE` erlaubt das Ändern von `DT_PROTECT` zur Laufzeit, ohne den Automaten zu verlassen – wirksam wird der neue Wert beim nächsten Eintritt in `UP_STOP`/`DOWN_STOP`.
- **Sofortiger Trip bei Konflikt:** Wie bei der Nicht-Adapter-Variante wird ein gleichzeitiger Befehl in beide Richtungen ohne Verzögerung erkannt.
- **Reset-Bedingung:** `EI_RESET` wirkt nur, wenn beide Eingangsadapter `D1 = FALSE` melden.

## Zustandsübersicht

| Zustand     | UP_OUT.D1 | DOWN_OUT.D1 | TRIP_OUT.D1 | Beschreibung                                            |
| ----------- | --------- | ----------- | ----------- | -------------------------------------------------------- |
| `STOP`      | FALSE     | FALSE       | FALSE       | Ruhezustand, keine Richtung aktiv.                       |
| `UP`        | TRUE      | FALSE       | FALSE       | Aufwärts-Richtung aktiv.                                 |
| `DOWN`      | FALSE     | TRUE        | FALSE       | Abwärts-Richtung aktiv.                                  |
| `UP_STOP`   | FALSE     | FALSE       | FALSE       | Wartet auf Ablauf von `DT_PROTECT` nach Freigabe von UP. |
| `DOWN_STOP` | FALSE     | FALSE       | FALSE       | Wartet auf Ablauf von `DT_PROTECT` nach Freigabe von DOWN. |
| `EVAL`      | –         | –           | –           | Kein Algorithmus; entscheidet über den Folgezustand.     |
| `TRIP`      | FALSE     | FALSE       | TRUE        | Konflikt/Trip, erfordert `EI_RESET`.                     |

## Anwendungsszenarien

- **Adapter-basierte Antriebssteuerungen:** Modulare Anlagen, in denen Richtungssignale bereits als `unidirectional::AX`-Adapter durch das System geführt werden.
- **Sicherheitsgerichtete Verriegelung mit Nachlaufzeit:** Anwendungen, die sowohl eine harte Konflikterkennung mit Quittierpflicht als auch eine Mindestpause zwischen Richtungswechseln benötigen.
- **Laufzeit-Parametrierung:** Systeme, in denen die Schutzzeit je nach Betriebszustand (z. B. Temperatur, Last) über `UPDATE` angepasst werden muss.

## Vergleich mit ähnlichen Bausteinen

Gegenüber `ILOCK_CONFLICT_TRIP_AX` ergänzt dieser Baustein die Zustände `UP_STOP`, `DOWN_STOP`, `EVAL` sowie den `timeOut`-Adapter und das `UPDATE`-Ereignis. Gegenüber `ILOCK_BLOCK_PROTECT_AX` unterscheidet er sich dadurch, dass ein gleichzeitiger Befehl in beide Richtungen nicht stillschweigend ignoriert, sondern als expliziter `TRIP`-Zustand behandelt wird, der ein `EI_RESET` erfordert. Gegenüber der Nicht-Adapter-Variante `ILOCK_CONFLICT_TRIP_PROTECT` ist die Schnittstelle vollständig adapterbasiert, was die Einbindung in modulare, adapterorientierte Systeme erleichtert.

## Fazit

`ILOCK_CONFLICT_TRIP_PROTECT_AX` vereint die Adapter-basierte Konflikterkennung mit Quittierpflicht und eine konfigurierbare Schutz-Totzeit in einem Baustein. Er eignet sich für modulare Automatisierungslösungen, die eine strikte gegenseitige Ausschließlichkeit zweier Richtungen mit Nachlaufzeit und Laufzeit-Parametrierbarkeit kombinieren müssen.
