# CEIL2_AR

![CEIL2_AR_network](./CEIL2_AR_network.svg)

* * * * * * * * * *

## Einleitung

`CEIL2_AR` ist ein Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::CEIL2` (DINT-Variante von CEIL für Werte außerhalb des INT-Bereichs). Er verbindet den `AR`-Adapter mit der OSCAT-Funktion `CEIL2` und gibt das Ergebnis als `ADI`-Adapter aus (kleinste ganze Zahl größer oder gleich X (DINT)). Der Wrapper überbrückt Adapter- und OSCAT-Bibliothek, ohne dass eine der beiden Basisbibliotheken von der anderen abhängt.

![CEIL2_AR](CEIL2_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine direkten Ereignis-Eingänge – Ereignisse kommen über den Adapter `AR_IN`.

### **Ereignis-Ausgänge**

Keine direkten Ereignis-Ausgänge – Ereignisse gehen über den Adapter `ADI_OUT`.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **AR_IN** (Socket, `adapter::types::unidirectional::AR`): Eingangssignal (`REAL`)
- **ADI_OUT** (Plug, `adapter::types::unidirectional::ADI`): kleinste ganze Zahl größer oder gleich X (DINT)

## Verwendete Funktionsbausteine (FBs)

- **`CEIL2`** (`OSCAT::Basic::POUs::Mathematical::CEIL2`): Rundungsfunktion, siehe [CEIL2](./CEIL2.md)
- **`E_D_FF_ANY_1`** (`iec61499::events::E_D_FF_ANY`): Änderungsfilter

## Funktionsweise

- `AR_IN.E1` → `CEIL2.REQ`; `AR_IN.D1` → `CEIL2.X`
- `CEIL2.CNF` → `E_D_FF_ANY_1.CLK`; Rückgabewert von `CEIL2` → `E_D_FF_ANY_1.D`
- `E_D_FF_ANY_1.EO` → `ADI_OUT.E1`; `E_D_FF_ANY_1.Q` → `ADI_OUT.D1`

## Technische Besonderheiten

- `E_D_FF_ANY_1` (Standardbibliothek `events-3.0.0`) filtert Änderungen: `ADI_OUT.E1` feuert nur, wenn sich das Ergebnis seit dem letzten Aufruf tatsächlich geändert hat. Gleiches Muster wie bei den Wrappern `FT_DERIV_AR`/`CTRL_PI_AR`.
- **Warum ein eigener Wrapper?** `CEIL2` ist eine reine OSCAT-`Function` ohne Adapter-Schnittstelle. `adapter` soll nicht von `OSCAT` abhängen (und umgekehrt); die Brücke liegt daher im dritten Bibliotheksprojekt `OSCAT_adapter`, das von beiden abhängt.

## Anwendungsszenarien

- Runden eines `AR`-Signals in einer Adapter-Kette auf eine ganze Zahl (`DINT`)

## Zusammenfassung

`CEIL2_AR` rundet einen `AR`-Wert mit `CEIL2` und gibt das Ergebnis nur bei Änderung als `ADI`-Adapter aus.
