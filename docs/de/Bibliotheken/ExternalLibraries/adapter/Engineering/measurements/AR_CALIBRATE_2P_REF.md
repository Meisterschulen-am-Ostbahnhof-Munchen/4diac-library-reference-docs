# AR_CALIBRATE_2P_REF

![AR_CALIBRATE_2P_REF_ecc](./AR_CALIBRATE_2P_REF_ecc.svg)

* * * * * * * * * *

## Einleitung

`AR_CALIBRATE_2P_REF` ist eine adapterbasierte 2-Punkt-Kalibrierung (Low/High). Beide Punkte werden unabhängig voneinander gespeichert, es gibt keine Reihenfolgeabhängigkeit wie bei `AR_CALIBRATE_SQ_REF`. Die Referenzziele (`Y_LOW`/`Y_HIGH`) sind lebende `AR2`-Sockets und werden über zwei weitere `AR2`-Plugs (`LOW_REF`/`HIGH_REF`) persistiert. Die Interpolation erfolgt erst live bei jedem `X.E1`.

![AR_CALIBRATE_2P_REF](AR_CALIBRATE_2P_REF.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Initialisierung bzw. Deinitialisierung. Verbundene Daten: `QI`, `xClipping`.
- `C_LOW`: Atomarer Trigger zum Kalibrieren des Low-Punkts (VT-Taste, OPC-UA-Methode oder Remote-Toggle nach Merge).
- `C_HIGH`: Atomarer Trigger zum Kalibrieren des High-Punkts.

### **Ereignis-Ausgänge**

- `INITO`: Initialisierungsbestätigung. Verbundene Daten: `QO`.

### **Daten-Eingänge**

| Name | Datentyp | Vorgabewert | Kommentar |
| :--- | :--- | :--- | :--- |
| `QI` | `BOOL` | | Eingangs-Qualifier (TRUE = aktivieren, FALSE = deaktivieren). |
| `xClipping` | `BOOL` | `TRUE` | TRUE: Ausgang wird auf `LOW_REF..HIGH_REF` geklemmt; FALSE: kein Clipping, Extrapolation über die Kalibrierpunkte hinaus erlaubt. |

### **Daten-Ausgänge**

- `QO` (`BOOL`): Ausgangs-Qualifier.

### **Adapter**

| Richtung | Name | Adaptertyp | Kommentar |
| :--- | :--- | :--- | :--- |
| **Plug** (Ausgang) | `Y` | `adapter::types::unidirectional::AR` | Kalibrierter Ausgang. |
| **Plug** (Ausgang) | `X_LOW` | `adapter::types::bidirectional::AR2` | Gespeicherter Rohwert am Low-Kalibrierpunkt. |
| **Plug** (Ausgang) | `X_HIGH` | `adapter::types::bidirectional::AR2` | Gespeicherter Rohwert am High-Kalibrierpunkt. |
| **Plug** (Ausgang) | `LOW_REF` | `adapter::types::bidirectional::AR2` | Gespeichertes Low-Ziel (Y_LOW). |
| **Plug** (Ausgang) | `HIGH_REF` | `adapter::types::bidirectional::AR2` | Gespeichertes High-Ziel (Y_HIGH). |
| **Socket** (Eingang) | `X` | `adapter::types::unidirectional::AR` | Rohwert. |
| **Socket** (Eingang) | `Y_LOW` | `adapter::types::bidirectional::AR2` | Ziel-Ausgangswert am Low-Punkt: Live-Eingabe über `EO1`/`DO1`, der gespeicherte bzw. wiederhergestellte Wert wird über `EI1`/`DI1` für VT/Web zurückgespiegelt. |
| **Socket** (Eingang) | `Y_HIGH` | `adapter::types::bidirectional::AR2` | Wie `Y_LOW`, für den High-Punkt. |

## Funktionsweise

Der Baustein ist ein Basis-FB. Im Zustand `IDLE` reagiert er auf folgende Ereignisse:

| Ereignis | Zustand | Aktion |
| :--- | :--- | :--- |
| `X.E1`, `X_LOW.EI1`, `X_HIGH.EI1` | `REQ` | Berechnet den kalibrierten Wert und gibt `Y.E1` aus. |
| `C_LOW` | `C_LOW` | `X_LOW.DO1 := X.D1`, Ausgabe `X_LOW.EO1`. |
| `C_HIGH` | `C_HIGH` | `X_HIGH.DO1 := X.D1`, Ausgabe `X_HIGH.EO1`. |
| `Y_LOW.EO1` | `LOW_REF_ECHO` | `LOW_REF.DO1 := Y_LOW.DO1`, Ausgabe `LOW_REF.EO1` (persistieren). |
| `Y_HIGH.EO1` | `HIGH_REF_ECHO` | `HIGH_REF.DO1 := Y_HIGH.DO1`, Ausgabe `HIGH_REF.EO1`. |
| `LOW_REF.EI1` | `Y_LOW_DISP` | `Y_LOW.DI1 := LOW_REF.DI1`, Ausgabe `Y_LOW.EI1` (Anzeige nach Boot und jeder Änderung). |
| `HIGH_REF.EI1` | `Y_HIGH_DISP` | `Y_HIGH.DI1 := HIGH_REF.DI1`, Ausgabe `Y_HIGH.EI1`. |

Alle Aktionszustände kehren unmittelbar nach `IDLE` zurück. Die Initialisierung läuft über `START` → `Init` (`INIT` mit `QI = TRUE`, `QO := QI`, `INITO`) → `IDLE`; `INIT` mit `QI = FALSE` führt aus `IDLE` über `DeInit` (`QO := FALSE`, `INITO`) zurück nach `START`.

**Berechnung im Zustand `REQ`:**

1. `QO := QI`. Nur bei `QI = TRUE` wird gerechnet.
2. Intern gelatchte Flags: `xLowCal` bzw. `xHighCal` werden TRUE, sobald `X_LOW.DI1` bzw. `X_HIGH.DI1` vom Default `0.0` abweicht.
3. Sind beide Punkte kalibriert: `Y.D1 = LOW_REF + (X - X_LOW) * (HIGH_REF - LOW_REF) / (X_HIGH - X_LOW)`; bei `X_HIGH = X_LOW` wird `LOW_REF` ausgegeben.
4. Ist nur Low kalibriert, wird `LOW_REF` ausgegeben; ist nur High kalibriert, `HIGH_REF`; ist nichts kalibriert, `LOW_REF`.
5. Clipping auf `[LOW_REF, HIGH_REF]` nur, wenn `xClipping` TRUE ist und beide Punkte kalibriert sind.

## Technische Besonderheiten

- **Keine Reihenfolgeabhängigkeit:** `C_LOW` und `C_HIGH` speichern ihren Rohwert unabhängig, jeder Punkt kann jederzeit einzeln neu kalibriert werden.
- **Teilkalibrierung:** Solange nur ein Punkt kalibriert ist, wird nicht gegen den noch auf `0.0` stehenden anderen Punkt interpoliert, sondern der bekannte REF-Wert ausgegeben.
- **Persistenz:** `X_LOW`, `X_HIGH`, `LOW_REF` und `HIGH_REF` sind bidirektionale `AR2`-Plugs; die dauerhafte Speicherung übernimmt ein nachgeschalteter Baustein (z. B. `INI_AR2`/`NVS_AR2`). Beim Boot zurückgelesene Werte erscheinen über `LOW_REF.EI1`/`HIGH_REF.EI1` wieder an `Y_LOW`/`Y_HIGH`.
- **Atomare Trigger:** `C_LOW`/`C_HIGH` sind reine Ereigniseingänge, kein `AX`-Adapter mit Flankenwächter.

## Anwendungsszenarien

- Kalibrierung eines Analogsensors an zwei Referenzpunkten (z. B. Null- und Endwert) mit VT-Eingabe der Zielwerte.
- Nachträgliches Nachtrimmen eines gedrifteten Nullpunkts, ohne den High-Punkt erneut zu kalibrieren.

## Zusammenfassung

`AR_CALIBRATE_2P_REF` kalibriert ein `AR`-Signal über zwei unabhängig gespeicherte Rohwerte und zwei live editierbare, persistierbare Referenzziele und gibt das linear interpolierte (optional geklemmte) Ergebnis über `Y` aus.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
