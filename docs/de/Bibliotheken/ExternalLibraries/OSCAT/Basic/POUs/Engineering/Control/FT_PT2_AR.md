# FT_PT2_AR

## Einleitung

`FT_PT2_AR` ist ein AR-Adapter-Wrapper um den OSCAT-Tiefpassfilterbaustein 2. Ordnung `FT_PT2`. Er ermöglicht die Filterung 2. Ordnung mit einstellbarer Zeitkonstante `TM`, Dämpfung `D` und Verstärkung `K` in rein adapterbasierten IEC 61499 Anwendungen.

Die Zeitkonstante `TM` wird über einen `ATM`-Adapter-Socket bereitgestellt (gemäß Section 13 des `iec61499-creator`-Skills), während Dämpfung `D` und Verstärkung `K` als gewöhnliche `InputVars` geführt werden. Ein internes `E_D_FF_ANY` D-Flipflop sorgt dafür, dass Ausgangsevent `AR_OUT.E1` beim ersten Zyklus bedingungslos und danach nur bei tatsächlichen Wertänderungen gesendet wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------- |
| `INIT` | `EInit` | Service-Initialisierung, wird an `FT_PT2.EINIT` durchgereicht |
| `RST` | `Event` | Setzt den Filterausgang über `FT_PT2.RST` zurück |

### **Ereignis-Ausgänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------- |
| `INITO` | `EInit` | Initialisierungsbestätigung von `FT_PT2` |

### **Daten-Eingänge**

| Name | Typ | Initialwert | Beschreibung |
| :--- | :--- | :------------ | :----------- |
| `D` | `REAL` | `0.0` | Dämpfungsfaktor des PT2-Filters (z. B. 0.707 für Butterworth-Charakteristik) |
| `K` | `REAL` | `1.0` | Verstärkungsfaktor |

### **Daten-Ausgänge**

Keine direkten Daten-Ausgänge vorhanden. Die Ausgabe erfolgt ausschließlich über den Adapter-Plug `AR_OUT`.

### **Adapter**

| Schnittstelle | Richtung | Adaptertyp | Datentyp | Beschreibung |
| :--- | :--- | :--- | :--- | :--- |
| `AR_IN` | Socket | `AR` | `REAL` | Eingangssignal für die PT2-Filterung |
| `TM` | Socket | `ATM` | `TIME` | Zeitkonstante des Tiefpassfilters (`TM.D1` an `FT_PT2.TM`) |
| `AR_OUT` | Plug | `AR` | `REAL` | Gefilterter Ausgangswert (`FT_PT2.out` via `E_D_FF_ANY`) |

## Funktionsweise

Der Baustein bettet `FT_PT2` in ein FBNetzwerk ein:

1. **Signalverarbeitung**:  
   Ein Ereignis auf `AR_IN.E1` löst `FT_PT2.REQ` aus. `AR_IN.D1` wird an `FT_PT2.in` weitergegeben.
2. **Parameterisierung**:  
   `TM.D1` wird vom `ATM`-Socket eingelesen. `D` und `K` werden direkt als Parameter übergeben.
3. **Zustandsbehaftete Integration 2. Ordnung**:  
   `FT_PT2` verarbeitet das Signal intern über zwei gekoppelte Integratoren (`INTEGRATE`).
4. **Änderungsfilterung (`E_D_FF_ANY`)**:  
   `FT_PT2.CNF` steuert das interne `E_D_FF_ANY`. Das Flipflop gibt beim ersten Zyklus den berechneten Ausgangswert an `AR_OUT` weiter. In Folgezyklen werden `AR_OUT.E1` und `AR_OUT.D1` nur aktualisiert, wenn der berechnete Filterwert vom bisherigen Wert abweicht.
5. **Reset & Initialisierung**:  
   `INIT` steuert `FT_PT2.EINIT`. `RST` setzt die internen Speicher der beiden Integrationsstufen zurück.

## Technische Besonderheiten

- **PT2-Charakteristik**: Bietet im Vergleich zum PT1-Filter eine stärkere Dämpfung höherfrequenter Störungen (12 dB/Oktave Abfall).
- **Integrierte Änderungsfilterung**: Vermeidet unnötige Adapter-Events in nachgelagerten SubApps.
- **`ATM`-Socket für Zeitparameter**: Konform zu den Projektdesignregeln für Zeit- und Konfigurationskonstanten.

## Anwendungsszenarien

- Filterung von hochfrequenten Störungen auf analogen Signalen (z. B. Kraft-, Druck- oder Fahrgeschwindigkeitssignalen).
- Schwingungsdämpfung in Regelkreisen.

## Siehe auch

- [`FT_PT2`](FT_PT2.md) – Grundbaustein (OSCAT).
- [`FT_PT1_AR`](FT_PT1_AR.md) – AR-Adapter-Wrapper für PT1-Filter 1. Ordnung.
- [`FT_DERIV_AR`](FT_DERIV_AR.md) – AR-Adapter-Wrapper für Differenzierer.
