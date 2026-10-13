# AR_CALIBRATE_3P_REF

![AR_CALIBRATE_3P_REF_ecc](./AR_CALIBRATE_3P_REF_ecc.svg)

![AR_CALIBRATE_3P_REF](AR_CALIBRATE_3P_REF.svg)

* * * * * * * * * *

## Einleitung

`AR_CALIBRATE_3P_REF` ist eine adapterbasierte Drei-Punkt-Kalibrierung für Joysticks und Sensoren mit Mittelstellung (Min, Mid, Max), in freier Reihenfolge wie [AR_CALIBRATE_3P](AR_CALIBRATE_3P.md). Die Referenz-Zielwerte (`Y_MIN`/`Y_MID`/`Y_MAX`) sind **lebende `AR2`-Sockets** und werden über drei zusätzliche `AR2`-Plugs (`MIN_REF`/`MID_REF`/`MAX_REF`) persistiert. Es gilt dasselbe Live-Echo-Muster wie bei `AR_CALIBRATE_SQ_REF` und [AR_CALIBRATE_2P_REF](AR_CALIBRATE_2P_REF.md).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung. Verknüpft mit `QI`, `xClipping`.
- **C_MIN**: Minimalpunkt kalibrieren, atomarer Trigger.
- **C_MID**: Mittelpunkt (Center) kalibrieren, atomarer Trigger.
- **C_MAX**: Maximalpunkt kalibrieren, atomarer Trigger.

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung. Verknüpft mit `QO`.

### **Daten-Eingänge**

- **QI** (`BOOL`): Ereignis-Eingangsqualifizierer
- **xClipping** (`BOOL`, Default `TRUE`): `TRUE` = Ausgang wird auf `MIN_REF..MAX_REF` geklemmt; `FALSE` = kein Clipping, Extrapolation über die Kalibrierpunkte hinaus erlaubt

### **Daten-Ausgänge**

- **QO** (`BOOL`): Ereignis-Ausgangsqualifizierer

### **Adapter**

Plugs:

- **Y** (`adapter::types::unidirectional::AR`): kalibrierter Ausgang
- **X_MIN**, **X_MID**, **X_MAX** (`adapter::types::bidirectional::AR2`): persistierter Rohwert am Min-/Mid-/Max-Kalibrierpunkt
- **MIN_REF**, **MID_REF**, **MAX_REF** (`adapter::types::bidirectional::AR2`): persistierte Zielreferenz (`Y_MIN`/`Y_MID`/`Y_MAX`)

Sockets:

- **X** (`adapter::types::unidirectional::AR`): Roheingang
- **Y_MIN**, **Y_MID**, **Y_MAX** (`adapter::types::bidirectional::AR2`): Zielausgabewert `Y` am Min-/Mid-/Max-Punkt. Live-Eingabe über `EO1`/`DO1` (Socket-seitiger Swap); der persistierte/wiederhergestellte Wert wird über `EI1`/`DI1` für VT-/Web-Anzeige zurückgespiegelt.

## Funktionsweise

Der Baustein startet im Zustand `START`; `INIT` mit `QI = TRUE` führt über `Init` nach `IDLE` (`QO := QI`), `INIT` mit `QI = FALSE` über `DeInit` zurück nach `START` (`QO := FALSE`). Aus `IDLE` sind folgende Aktionszustände erreichbar, die sofort nach `IDLE` zurückkehren:

- **REQ** (bei `X.E1` oder einem Ereignis `X_MIN.EI1`/`X_MID.EI1`/`X_MAX.EI1`): berechnet `Y` und feuert `Y.E1`
- **C_MIN**/**C_MID**/**C_MAX**: speichern den Roheingang `X.D1` in `X_MIN.DO1`/`X_MID.DO1`/`X_MAX.DO1` und feuern `X_MIN.EO1`/`X_MID.EO1`/`X_MAX.EO1`
- **MIN_REF_ECHO**/**MID_REF_ECHO**/**MAX_REF_ECHO** (bei `Y_MIN.EO1`/`Y_MID.EO1`/`Y_MAX.EO1`): übertragen die Live-Referenz `Y_xxx.DO1` nach `xxx_REF.DO1` (Persistenz)
- **Y_MIN_DISP**/**Y_MID_DISP**/**Y_MAX_DISP** (bei `MIN_REF.EI1`/`MID_REF.EI1`/`MAX_REF.EI1`): spiegeln die persistierte/wiederhergestellte Referenz nach `Y_xxx.DI1` zurück (nach Boot und nach jeder Änderung)

Formel: stückweise lineare Interpolation zwischen `(X_MIN, MIN_REF)`, `(X_MID, MID_REF)` und `(X_MAX, MAX_REF)`, auf `MIN_REF..MAX_REF` geklemmt, sofern `xClipping = TRUE`.

**Teilkalibrierung (bevor alle drei Punkte gesetzt sind):** Jeder Punkt wird intern verfolgt (`xMinCal`/`xMidCal`/`xMaxCal`, gelatcht auf TRUE, sobald sein Rohwert vom INI-Default `0.0` abweicht; bleibt über Neustarts korrekt gesetzt, da Boot-Restore den persistierten Wert zurückliefert). Bis alle drei kalibriert sind, fällt `REQ` sauber zurück, statt auf einem noch unkalibrierten `X_MID.DI1` (bei `0.0`) zu pivotieren, was den MIN-Zweig dauerhaft unerreichbar machen würde:

- Nur ein Punkt kalibriert: `Y` = dessen eigener REF-Wert (keine Interpolation möglich)
- Zwei Punkte kalibriert: Zwei-Punkt-Gerade zwischen genau diesen beiden, der dritte wird ignoriert
- Alle drei kalibriert: volle stückweise Interpolation
- Keiner kalibriert: `Y := MID_REF`

Geklemmt wird nur, wenn `xClipping` gesetzt ist und sowohl MIN als auch MAX kalibriert sind; andernfalls würde die noch unkalibrierte Grenze mit dem INI-Default `0.0` den Teilkalibrierungs-Fallback verfälschen. MID ist dafür irrelevant.

**Kalibrierung (Drei-Punkt, freie Reihenfolge):**

1. **Min (`C_MIN`)**: Minimum anfahren, `Y_MIN` setzen, `C_MIN` auslösen → `X_MIN := X`
2. **Mid (`C_MID`)**: Mitte anfahren, `Y_MID` setzen, `C_MID` auslösen → `X_MID := X`
3. **Max (`C_MAX`)**: Maximum anfahren, `Y_MAX` setzen, `C_MAX` auslösen → `X_MAX := X`

## Technische Besonderheiten

- **Freie Reihenfolge** wie bei `AR_CALIBRATE_3P`: Jeder Punkt wird unabhängig gespeichert und kann einzeln neu kalibriert werden (z. B. nur die Mitte nachtrimmen), ohne Datenabhängigkeit zwischen den Kalibrieraktionen.
- **Referenzen als Live-Sockets:** `Y_MIN`/`Y_MID`/`Y_MAX` sind bidirektionale `AR2`-Sockets, gespeist von einem externen VT-/Web-Leser, nach außen über die `AR2`-Plugs `MIN_REF`/`MID_REF`/`MAX_REF` für die Persistenz gespiegelt (`INI_AR2`/`NVS_AR2`) und über `MIN_REF.EI1`/`MID_REF.EI1`/`MAX_REF.EI1` zurückgespielt, sodass ein nach dem Boot wiederhergestellter Wert am Bildschirm erscheint, ohne dass der Bediener ihn erneut eingeben muss.
- Entartete Punkte (`X_MID <= X_MIN` bzw. `X_MAX <= X_MID`) liefern den jeweiligen REF-Wert statt einer Division durch Null.

## Zustandsübersicht

```text
START --INIT[QI]--> Init --1--> IDLE
IDLE --INIT[!QI]--> DeInit --1--> START
IDLE --X.E1 / X_xxx.EI1--> REQ --1--> IDLE
IDLE --C_MIN / C_MID / C_MAX--> C_xxx --1--> IDLE
IDLE --Y_xxx.EO1--> xxx_REF_ECHO --1--> IDLE
IDLE --xxx_REF.EI1--> Y_xxx_DISP --1--> IDLE
```

## Anwendungsszenarien

- Lenkwinkelsensor: Min = Vollanschlag links, Mid = geradeaus, Max = Vollanschlag rechts. `MIN_REF`/`MID_REF`/`MAX_REF` enthalten die gewünschten kalibrierten Winkelwerte (z. B. -30,0 / 0,0 / 30,0 Grad), `X_MIN`/`X_MID`/`X_MAX` die tatsächlich gemessenen Rohwerte.
- Joysticks mit Mittelstellung und persistierten, am VT editierbaren Referenzwerten

## ⚖️ Vergleich mit ähnlichen Bausteinen

- [AR_CALIBRATE_3P](AR_CALIBRATE_3P.md): Drei-Punkt-Kalibrierung ohne `*_REF`-Sockets
- [AR_CALIBRATE_2P_REF](AR_CALIBRATE_2P_REF.md): Zwei-Punkt-Variante mit denselben Live-Referenzen

## Fazit

`AR_CALIBRATE_3P_REF` kalibriert drei Punkte in freier Reihenfolge, hält Rohwerte und Referenzen persistiert über `AR2`-Adapter und fällt bei Teilkalibrierung sauber zurück.
