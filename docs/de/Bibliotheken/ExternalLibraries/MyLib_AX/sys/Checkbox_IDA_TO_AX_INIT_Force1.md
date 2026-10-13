# Checkbox_IDA_TO_AX_INIT_Force1

![Checkbox_IDA_TO_AX_INIT_Force1_network](./Checkbox_IDA_TO_AX_INIT_Force1_network.svg)

* * * * * * * * * *

## Einleitung

`Checkbox_IDA_TO_AX_INIT_Force1` fasst `Checkbox_IDA_TO_AX_INIT` (boot-stabiler AX-Pegel, startet `TRUE` bis zur ersten echten VT-Änderung) und `Q_NumericValue` (erzwingt beim FORTE-Start einmalig den Wert 1 der zugehörigen NumberVariable) zusammen. Beide Bausteine lesen bzw. schreiben dieselbe `u16ObjId`, daher gehören sie zusammen; die `u16ObjId` ist als Parameter wiederverwendbar. Die SubApp wurde aus `Aufnahme_Automatik.SUB` herausgelöst (`Permit_Automatik` plus `Q_NumericValue_Force1_Geschwindigkeitsregelung`).

![Checkbox_IDA_TO_AX_INIT_Force1](Checkbox_IDA_TO_AX_INIT_Force1.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden*

### **Ereignis-Ausgänge**

*Keine direkten Ereignis-Ausgänge vorhanden*

### **Daten-Eingänge**

- **u16ObjId** (`UINT`, Vorgabe `ID_NULL`): Object-ID der VT-Checkbox-NumberVariable (dasselbe Objekt für Lesen und Force1-Schreiben)

### **Daten-Ausgänge**

*Keine Daten-Ausgänge vorhanden*

### **Adapter**

- **PERMIT_OUT** (Plug, `adapter::types::unidirectional::AX`): Boot-stabiler Checkbox-Zustand (startet `TRUE`, folgt danach der echten VT-Checkbox)

## Verwendete Funktionsbausteine (FBs)

- **Reader**: SubApp `MyLib_AX::sys::Checkbox_IDA_TO_AX_INIT` – liefert den boot-stabilen Checkbox-Zustand als AX
- **Writer**: `isobus::UT::Q::Q_NumericValue` mit `u32NewValue=1` – fester FB-seitiger Einmalschreiber (nicht die `InitialValue`-Property des VT-Pools)

## Programmablauf und Verbindungen

- `u16ObjId` → `Reader.u16ObjId` und `Writer.u16ObjId`
- `Writer.INITO` → `Writer.REQ`: Beim Start wird einmalig der Wert 1 geschrieben.
- `Reader.PERMIT_OUT` → `PERMIT_OUT`

## Technische Besonderheiten

- Der Writer löst durch die Rückführung `INITO` → `REQ` genau einmal beim Initialisieren aus; danach bleibt die Checkbox unter der Kontrolle des Anwenders.

## Anwendungsszenarien

- Freigabe-Checkbox (z. B. für eine Automatik), die beim Start aktiv sein soll und deren NumberVariable beim FORTE-Start konsistent auf 1 gesetzt wird

## Fazit

`Checkbox_IDA_TO_AX_INIT_Force1` stellt eine beim Start aktive, boot-stabile Checkbox-Freigabe als AX bereit und setzt das zugehörige VT-Objekt konsistent auf 1.
