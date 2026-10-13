# logiBUS_AI_Raw_Publish_OPC

![logiBUS_AI_Raw_Publish_OPC_network](./logiBUS_AI_Raw_Publish_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_AI_Raw_Publish_OPC` liest einen Analog-Eingang (`logiBUS_AI_IDA`) als Rohwert (DWORD, 0-4095) und publiziert ihn per OPC-UA, **ohne jegliche VT-Interaktion**. Er ist für Sensoren gedacht, deren physischer Analog-Eingang auf einer Station ohne VT-Anschluss sitzt (z. B. Hangausgleich-Winkelsensor B14 auf APIXON STG3). Die Kalibrierung sowie VT-Taster und -Anzeigen laufen dann auf der Station mit VT-Anschluss und werden per Remote-Subscribe dieses Rohwerts gespeist (siehe [`logiBUS_AI_Calibrate_3P_Remote_IDA_OPC`](logiBUS_AI_Calibrate_3P_Remote_IDA_OPC.md)).

![logiBUS_AI_Raw_Publish_OPC](logiBUS_AI_Raw_Publish_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

- **Input** (`logiBUS::io::AI::logiBUS_AI_S`): Auswahl des Analog-Eingangs `AnalogInput_I1`..`I8` (Default `logiBUS_AI::Invalid`)
- **ID_RAW_WRITE** (WSTRING): Publish-Adresse des Rohwerts (DWORD 0-4095, `ACTION=PUBLISH`), lokal auf dieser Station

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Das Netzwerk besteht aus zwei Bausteinen:

- `logiBUS_AI_IDA` (`logiBUS::io::AI::logiBUS_AI_IDA`, `QI = TRUE`): liest den gewählten Analog-Eingang (`Input` → `logiBUS_AI_IDA.Input`). Parametriert sind `AnalogInput_hysteresis = DWORD#50`, `TimeDelta = DWORD#250` und `TimeRateLimit = DWORD#100`.
- `AD_PUBLISH_RAW` (`adapter::net::AD_PUBLISH_1`, `QI = TRUE`): `logiBUS_AI_IDA.IN` → `AD_PUBLISH_RAW.IN`; publiziert den Rohwert an die Adresse `ID_RAW_WRITE` (`ID_RAW_WRITE` → `AD_PUBLISH_RAW.ID`).

## Technische Besonderheiten

- Rohwert-Hälfte von `logiBUS_AI_Calibrate_3P_IDA_OPC`: Die Kalibrierung findet nicht hier, sondern auf der Station mit VT-Anschluss statt
- Keine Ereignis-, Daten- oder Adapter-Ports außer den beiden Eingangsvariablen
- Das Verhalten des Rohwert-Eingangs (Hysterese, `TimeDelta`, `TimeRateLimit`) wird über die Parameter von `logiBUS_AI_IDA` festgelegt

## Anwendungsszenarien

- Sensoren an Stationen ohne VT-Anschluss, deren Kalibrierung und Anzeige auf einer anderen Station laufen

## Fazit

`logiBUS_AI_Raw_Publish_OPC` stellt den Rohwert eines Analog-Eingangs per OPC-UA bereit und ist damit die Sensor-Seite einer stationsübergreifenden Kalibrierung.
