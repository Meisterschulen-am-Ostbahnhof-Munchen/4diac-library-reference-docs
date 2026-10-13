# CEIL_AR

![CEIL_AR_network](./CEIL_AR_network.svg)

* * * * * * * * * *

## Einleitung

`CEIL_AR` ist ein Adapter-Wrapper um `OSCAT::Basic::POUs::Mathematical::CEIL`. Er verbindet den `AR`-Adapter mit der OSCAT-Funktion `CEIL` und gibt das Ergebnis als `AI`-Adapter aus (kleinste ganze Zahl größer oder gleich X (INT)). Der Wrapper überbrückt Adapter- und OSCAT-Bibliothek, ohne dass eine der beiden Basisbibliotheken von der anderen abhängt.

![CEIL_AR](CEIL_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine direkten Ereignis-Eingänge – Ereignisse kommen über den Adapter `AR_IN`.

### **Ereignis-Ausgänge**

Keine direkten Ereignis-Ausgänge – Ereignisse gehen über den Adapter `AI_OUT`.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **AR_IN** (Socket, `adapter::types::unidirectional::AR`): Eingangssignal (`REAL`)
- **AI_OUT** (Plug, `adapter::types::unidirectional::AI`): kleinste ganze Zahl größer oder gleich X (INT)

## Verwendete Funktionsbausteine (FBs)

- **`CEIL`** (`OSCAT::Basic::POUs::Mathematical::CEIL`): Rundungsfunktion, siehe [CEIL](./CEIL.md)
- **`E_D_FF_ANY_1`** (`iec61499::events::E_D_FF_ANY`): Änderungsfilter

## Funktionsweise

- `AR_IN.E1` → `CEIL.REQ`; `AR_IN.D1` → `CEIL.X`
- `CEIL.CNF` → `E_D_FF_ANY_1.CLK`; Rückgabewert von `CEIL` → `E_D_FF_ANY_1.D`
- `E_D_FF_ANY_1.EO` → `AI_OUT.E1`; `E_D_FF_ANY_1.Q` → `AI_OUT.D1`

## Technische Besonderheiten

- `E_D_FF_ANY_1` (Standardbibliothek `events-3.0.0`) filtert Änderungen: `AI_OUT.E1` feuert nur, wenn sich das Ergebnis seit dem letzten Aufruf tatsächlich geändert hat. Gleiches Muster wie bei den Wrappern `FT_DERIV_AR`/`CTRL_PI_AR`.
- **Warum ein eigener Wrapper?** `CEIL` ist eine reine OSCAT-`Function` ohne Adapter-Schnittstelle. `adapter` soll nicht von `OSCAT` abhängen (und umgekehrt); die Brücke liegt daher im dritten Bibliotheksprojekt `OSCAT_adapter`, das von beiden abhängt.

## Anwendungsszenarien

- Runden eines `AR`-Signals in einer Adapter-Kette auf eine ganze Zahl (`INT`)

## Zusammenfassung

`CEIL_AR` rundet einen `AR`-Wert mit `CEIL` und gibt das Ergebnis nur bei Änderung als `AI`-Adapter aus.
