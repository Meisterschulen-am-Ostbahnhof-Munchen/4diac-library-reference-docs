# FT_PT1_AR

## Einleitung

`FT_PT1_AR` ist ein AR-Adapter-Wrapper um den OSCAT-Tiefpassfilterbaustein `FT_PT1`. Er kapselt die Tiefpassfilterung 1. Ordnung hinter einer rein adapterbasierten Schnittstelle für IEC 61499 Anwendungen.

Die Zeitkonstante `TM` ist als `ATM`-Socket ausgeführt (gemäß Section 13 des `iec61499-creator`-Skills), während der Verstärkungsfaktor `K` als gewöhnliche `InputVar` geführt wird. Der Baustein verwendet intern ein `E_D_FF_ANY` D-Flipflop, um Änderungen am gefilterten Wert zu erkennen und Adapter-Events nur bei tatsächlichen Wertänderungen (bzw. beim ersten Aufruf nach dem Start) zu senden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------- |
| `INIT` | `EInit` | Service-Initialisierung, wird an `FT_PT1.INIT` durchgereicht |
| `RST` | `Event` | Invalidiert den Filterzustand über `FT_PT1.RST` (`init := FALSE`) |

### **Ereignis-Ausgänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------- |
| `INITO` | `EInit` | Initialisierungsbestätigung von `FT_PT1` |

### **Daten-Eingänge**

| Name | Typ | Initialwert | Beschreibung |
| :--- | :--- | :------------ | :----------- |
| `K` | `REAL` | `1.0` | Verstärkungsfaktor, wird an `FT_PT1.K` durchgereicht |

### **Daten-Ausgänge**

Keine direkten Daten-Ausgänge vorhanden. Die Ausgabe erfolgt ausschließlich über den Adapter-Plug `AR_OUT`.

### **Adapter**

| Schnittstelle | Richtung | Adaptertyp | Datentyp | Beschreibung |
| :--- | :--- | :--- | :--- | :--- |
| `AR_IN` | Socket | `AR` | `REAL` | Eingangssignal für die Filterung |
| `TM` | Socket | `ATM` | `TIME` | Zeitkonstante des Tiefpassfilters (`TM.D1` wird an `FT_PT1.TM` weitergeleitet) |
| `AR_OUT` | Plug | `AR` | `REAL` | Gefilterter Ausgangswert (`FT_PT1.out` via `E_D_FF_ANY`) |

## Funktionsweise

Der Baustein verbindet `FT_PT1` (OSCAT) und `E_D_FF_ANY` in einem internen Netzwerk:

1. **Messwertverarbeitung**:  
   Ein Ereignis auf `AR_IN.E1` löst `FT_PT1.REQ` aus. `AR_IN.D1` liefert den aktuellen Messwert.
2. **Zeitkonstante & Reinitialisierung**:  
   Die Filterzeit `TM.D1` kommt vom `ATM`-Socket und wird an `FT_PT1.TM` übergeben. Ein Ereignis auf `TM.E1` löst zusätzlich intern direkt `FT_PT1.RST` aus. Dadurch wird der Filterzustand invalidiert (`init := FALSE`), sodass beim nachfolgenden `AR_IN.E1`-Ereignis der Filter mit frischem Eingangssignal (`out := K * in`) und der neuen Zeitkonstante sauber reinitialisiert wird.
3. **Änderungserkennung & Entkopplung (`E_D_FF_ANY`)**:  
   Nach der Berechnung feuert `FT_PT1.CNF` den `CLK`-Eingang des internen `E_D_FF_ANY`. Das Flipflop gibt beim ersten Aufruf nach dem Start den gefilterten Ausgangswert bedingungslos über `AR_OUT.E1` / `AR_OUT.D1` aus. Bei nachfolgenden Zyklen werden Ereignisse nur dann gefeuert, wenn sich der gefilterte Wert tatsächlich vom vorherigen Ausgangswert unterscheidet.
4. **Reset & Initialisierung**:  
   - `INIT` steuert `FT_PT1.INIT` und meldet Vollzug über `INITO`. Unverdrahtete `INIT`-Events feuern beim Deployment automatisch einmalig.
   - `RST` sowie jedes eintreffende `TM.E1`-Ereignis werden an `FT_PT1.RST` weitergeleitet, um den Filterzustand zu invalidieren (`init := FALSE`). Die Reinitialisierung erfolgt beim nächsten `AR_IN.E1`-Aufruf.

## Technische Besonderheiten

- **Saubere Adaptergrenze**: Verhindert das direkte Zugreifen auf interne `.E1`/`.D1`-Datenstrukturen in SubApp-Netzwerken.
- **Automatische Filter-Reinitialisierung bei TM-Änderung**: Ein Eintreffen von `TM.E1` löst intern `FT_PT1.RST` aus (setzt `init := FALSE`). Beim nächsten `AR_IN.E1`-Aufruf wird `out` mit dem aktuellen Messwert (`K * in`) reinitialisiert und der Zeitstempel aktualisiert, damit geänderte Filterzeiten ohne Sprungartefakte oder veraltete Seeding-Werte sauber wirksam werden.
- **Entlastung der Ereigniskette**: `E_D_FF_ANY` verhindert unnötiges Event-Spamming in nachgelagerten SubApps bei unveränderten Werten.
- **`ATM`-Socket-Konvention**: Zeitparameter werden nicht als nackte Variablen deklariert, sondern über `ATM`-Adapter-Sockets eingebunden (an der Instanziierungsstelle z. B. über `initval_ATM` zu speisen).

## Anwendungsszenarien

- Glättung von analogen Sensorwerten (z. B. Druck, Temperatur, Drehzahl) in adapterbasierten SubApp-Architekturen.
- Rauschfilterung vor Schwellwert- oder Hysterese-Bausteinen (`AR_D_FF_HYS_TMIN`).

## Siehe auch

- [`FT_PT1`](FT_PT1.md) – Grundbaustein (OSCAT).
- [`FT_PT2_AR`](FT_PT2_AR.md) – AR-Adapter-Wrapper für PT2-Filter 2. Ordnung.
- [`FT_DERIV_AR`](FT_DERIV_AR.md) – AR-Adapter-Wrapper für Differenzierer.
