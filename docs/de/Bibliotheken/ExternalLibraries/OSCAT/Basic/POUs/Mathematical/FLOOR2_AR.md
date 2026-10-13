# FLOOR2_AR

![FLOOR2_AR_network](./FLOOR2_AR_network.svg)

* * * * * * * * * *

## Einleitung

`FLOOR2_AR` ist ein AR/ADI-Adapter-Wrapper um die OSCAT-Funktion [`OSCAT::Basic::POUs::Mathematical::FLOOR2`](FLOOR2.md) (größte ganze Zahl `<= X`, `DINT`-Variante von `FLOOR` für Werte außerhalb des `INT`-Bereichs). Er überbrückt zwischen Adaptern und OSCAT, ohne dass eine der beiden Basisbibliotheken von der anderen abhängt.

![FLOOR2_AR](FLOOR2_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

Der Funktionsblock verfügt über keine direkten Daten-Eingänge.

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **AR_IN** (Socket, `adapter::types::unidirectional::AR`): Eingangssignal (REAL)
- **ADI_OUT** (Plug, `adapter::types::unidirectional::ADI`): größte ganze Zahl `<= X` (DINT)

## Funktionsweise

- `AR_IN.E1` → `FLOOR2.REQ` und `AR_IN.D1` → `FLOOR2.X`: Jedes neue Eingangssignal startet die Berechnung.
- `FLOOR2.CNF` → `E_D_FF_ANY_1.CLK`; das Funktionsergebnis geht an `E_D_FF_ANY_1.D`.
- `E_D_FF_ANY_1` ([`iec61499::events::E_D_FF_ANY`](../../../../../StandardLibraries/events/E_D_FF_ANY.md)) filtert Änderungen: `E_D_FF_ANY_1.EO` → `ADI_OUT.E1` und `E_D_FF_ANY_1.Q` → `ADI_OUT.D1`, aber nur, wenn sich das Ergebnis seit dem letzten Aufruf tatsächlich geändert hat.

## Technische Besonderheiten

- Änderungsfilter: `ADI_OUT.E1` feuert nur bei tatsächlicher Wertänderung (gleiches Muster wie bei `CTRL_PI_AR`/`FT_DERIV_AR`)
- `FLOOR2` ist eine reine OSCAT-Funktion ohne Adapter-Schnittstelle; `adapter` soll nicht von `OSCAT` abhängen (und umgekehrt), die Brücke liegt daher im dritten Bibliotheksprojekt `OSCAT_adapter`

## Anwendungsszenarien

- Abrunden eines adapterbasierten REAL-Werts auf eine ganze Zahl (DINT) in einem Adapter-Netzwerk

## ⚖️ Vergleich mit ähnlichen Bausteinen

`ROUND_AR` rundet auf Nachkommastellen (REAL-Ausgang); `FLOOR2_AR` liefert die abgerundete ganze Zahl als DINT.

## Fazit

`FLOOR2_AR` stellt `FLOOR2` als Adapter-Baustein von `AR` nach `ADI` bereit und gibt das Ergebnis nur bei Änderung aus.
