# ILOCK_CONFLICT_TRIP_PROTECT

![ILOCK_CONFLICT_TRIP_PROTECT](./ILOCK_CONFLICT_TRIP_PROTECT.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `ILOCK_CONFLICT_TRIP_PROTECT` erweitert `ILOCK_CONFLICT_TRIP` um eine konfigurierbare Schutz-Totzeit (`DT_PROTECT`), analog zu der Art, wie `ILOCK_BLOCK_PROTECT` das Verhalten von `ILOCK_BLOCK` ergänzt. Er priorisiert weiterhin den zuerst aktiven Eingang, löst bei gleichzeitiger Aktivierung beider Richtungen einen Trip-Zustand aus und benötigt danach ein explizites `EI_RESET`. Neu ist, dass nach Freigabe des aktiven Eingangs zunächst die Totzeit `DT_PROTECT` verstreichen muss, bevor der Baustein die aktuellen Eingangssignale neu bewertet – erst danach wird die nächste Richtung (oder gegebenenfalls ein Trip) übernommen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name       | Mitgeführte Daten          | Beschreibung                                                                 |
| ---------- | --------------------------- | ----------------------------------------------------------------------------- |
| `EI_UP`    | `DI_UP`, `DT_PROTECT`       | Ereignis für die Aufwärts-/Vorwärts-Richtung.                                |
| `EI_DOWN`  | `DI_DOWN`, `DT_PROTECT`     | Ereignis für die Abwärts-/Rückwärts-Richtung.                                |
| `EI_RESET` | `DI_UP`, `DI_DOWN`          | Rücksetzen des Trip-Zustands; nur wirksam, wenn beide Daten-Eingänge FALSE sind. |

### **Ereignis-Ausgänge**

| Name       | Mitgeführte Daten | Beschreibung                              |
| ---------- | ------------------ | ------------------------------------------ |
| `EO_UP`    | `DO_UP`            | Wird ausgelöst, wenn die UP-Richtung aktiv oder deaktiviert wird. |
| `EO_DOWN`  | `DO_DOWN`          | Wird ausgelöst, wenn die DOWN-Richtung aktiv oder deaktiviert wird. |
| `EO_TRIP`  | `DO_TRIP`          | Wird im TRIP-Zustand mitgesendet und signalisiert den Trip-Status. |

### **Daten-Eingänge**

- `DI_UP` (BOOL) – TRUE = vorwärts/aufwärts/rechts/im Uhrzeigersinn.
- `DI_DOWN` (BOOL) – TRUE = rückwärts/abwärts/links/gegen den Uhrzeigersinn.
- `DT_PROTECT` (TIME, Initialwert `T#50ms`) – Schutz-Totzeit, die nach der Freigabe des aktiven Eingangs verstreichen muss, bevor eine erneute Bewertung erfolgt.

### **Daten-Ausgänge**

- `DO_UP` (BOOL) – Signalisiert die aktive UP-Richtung.
- `DO_DOWN` (BOOL) – Signalisiert die aktive DOWN-Richtung.
- `DO_TRIP` (BOOL) – Signalisiert den Trip-/Konfliktzustand.

### **Adapter**

| Adapter   | Typ                           | Richtung | Beschreibung                                                                                     |
| --------- | ------------------------------ | -------- | -------------------------------------------------------------------------------------------------- |
| `timeOut` | `iec61499::events::ATimeOut`   | Plug     | Timer-Adapter für die Schutzzeit. Der Baustein setzt `timeOut.DT` und startet ihn über `timeOut.START`; `timeOut.TimeOut` signalisiert den Ablauf. |

## Funktionsweise

Der Baustein arbeitet als endlicher Automat (ECC) mit sieben Zuständen:

1. **STOP** – Ruhezustand, alle Ausgänge FALSE. Bei `EI_UP` mit `DI_UP AND NOT DI_DOWN` → **UP**; bei `EI_DOWN` mit `DI_DOWN AND NOT DI_UP` → **DOWN**; sind bei `EI_UP` oder `EI_DOWN` beide Datensignale TRUE → sofort **TRIP**.
2. **UP** – `DO_UP = TRUE`. Bei `EI_UP[NOT DI_UP]` → **UP_STOP**; bei `EI_DOWN[DI_DOWN]` (Konflikt während aktivem UP) → sofort **TRIP**.
3. **DOWN** – `DO_DOWN = TRUE`. Bei `EI_DOWN[NOT DI_DOWN]` → **DOWN_STOP**; bei `EI_UP[DI_UP]` (Konflikt während aktivem DOWN) → sofort **TRIP**.
4. **UP_STOP** / **DOWN_STOP** – Übergangszustände nach Freigabe des aktiven Eingangs. Der Algorithmus `STOP` setzt alle Ausgänge auf FALSE, überträgt `DT_PROTECT` an `timeOut.DT` und startet den Timer (`timeOut.START`). Bei Ablauf (`timeOut.TimeOut`) → **EVAL**.
5. **EVAL** – Kein eigener Algorithmus. Die aktuellen Werte von `DI_UP`/`DI_DOWN` werden neu bewertet: nur `DI_UP` TRUE → **UP**; nur `DI_DOWN` TRUE → **DOWN**; beide FALSE → **STOP**; beide TRUE → **TRIP**.
6. **TRIP** – `DO_TRIP = TRUE`, alle anderen Ausgänge FALSE. Verlassen nur über `EI_RESET`, wenn `NOT DI_UP AND NOT DI_DOWN` → **STOP**.

Die Trip-Erkennung selbst erfolgt weiterhin sofort und unabhängig von der Totzeit: Ein Konflikt während `STOP`, `UP` oder `DOWN` löst immer direkt `TRIP` aus. Die Totzeit `DT_PROTECT` wirkt ausschließlich zwischen der Freigabe eines aktiven Eingangs und der Übernahme der nächsten Richtung.

## Technische Besonderheiten

- **Kombination zweier Muster:** Der Baustein verbindet die Trip-bei-Konflikt-Logik von `ILOCK_CONFLICT_TRIP` mit der Totzeit-Logik von `ILOCK_BLOCK_PROTECT`.
- **Sofortiger Trip, verzögerte Freigabe:** Konflikte werden ohne Verzögerung erkannt; nur die Rückkehr in einen neuen gültigen Zustand nach Freigabe wird durch `DT_PROTECT` verzögert.
- **Reset-Bedingung:** `EI_RESET` wirkt nur, wenn beide Dateneingänge inaktiv sind – ein Reset bei fortbestehendem Konflikt bleibt wirkungslos.
- **Timer-Neustart in jedem Stop-Zwischenzustand:** `timeOut.DT` wird bei jedem Eintritt in `UP_STOP`/`DOWN_STOP` neu aus `DT_PROTECT` übernommen, sodass eine Änderung des Parameters unmittelbar in der nächsten Verzögerung wirksam wird.

## Zustandsübersicht

| Zustand     | DO_UP | DO_DOWN | DO_TRIP | Beschreibung                                            |
| ----------- | ----- | ------- | ------- | -------------------------------------------------------- |
| `STOP`      | FALSE | FALSE   | FALSE   | Ruhezustand, keine Richtung aktiv.                       |
| `UP`        | TRUE  | FALSE   | FALSE   | Aufwärts-Richtung aktiv.                                 |
| `DOWN`      | FALSE | TRUE    | FALSE   | Abwärts-Richtung aktiv.                                  |
| `UP_STOP`   | FALSE | FALSE   | FALSE   | Wartet auf Ablauf von `DT_PROTECT` nach Freigabe von UP. |
| `DOWN_STOP` | FALSE | FALSE   | FALSE   | Wartet auf Ablauf von `DT_PROTECT` nach Freigabe von DOWN. |
| `EVAL`      | –     | –       | –       | Kein Algorithmus; entscheidet anhand aktueller Eingänge über den Folgezustand. |
| `TRIP`      | FALSE | FALSE   | TRUE    | Konflikt/Trip, erfordert `EI_RESET`.                     |

## Anwendungsszenarien

- **Antriebe mit Nachlauf:** Wenn nach dem Loslassen eines Fahrbefehls eine mechanische oder hydraulische Nachlaufzeit eingehalten werden muss, bevor sicher in die Gegenrichtung geschaltet werden darf.
- **Sicherheitsgerichtete Verriegelung mit Quittierpflicht:** Anwendungen, in denen ein gleichzeitiger Befehl in beide Richtungen als Fehler gilt und explizit quittiert werden muss.
- **Ventil- oder Klappensteuerungen:** Schutz vor Druckstößen oder mechanischer Überlastung durch eine Mindestpause zwischen Richtungswechseln.

## Vergleich mit ähnlichen Bausteinen

Gegenüber `ILOCK_CONFLICT_TRIP` fügt dieser Baustein die Zustände `UP_STOP`, `DOWN_STOP` und `EVAL` sowie den `timeOut`-Adapter hinzu – die Trip-Logik selbst bleibt identisch. Gegenüber `ILOCK_BLOCK_PROTECT` unterscheidet er sich dadurch, dass ein gleichzeitiger Befehl in beide Richtungen nicht ignoriert, sondern als expliziter Fehlerzustand (`TRIP`) behandelt wird, der ein `EI_RESET` erfordert.

## Fazit

`ILOCK_CONFLICT_TRIP_PROTECT` verbindet die klare Fehlererkennung von `ILOCK_CONFLICT_TRIP` mit der Schutz-Totzeit von `ILOCK_BLOCK_PROTECT`. Er eignet sich für Anwendungen, die sowohl eine harte Konflikterkennung mit Quittierpflicht als auch eine Mindestpause zwischen Richtungswechseln benötigen.
