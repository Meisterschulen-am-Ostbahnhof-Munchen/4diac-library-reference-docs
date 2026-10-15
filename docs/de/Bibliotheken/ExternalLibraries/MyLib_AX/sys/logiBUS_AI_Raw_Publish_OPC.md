# logiBUS_AI_Raw_Publish_OPC

![logiBUS_AI_Raw_Publish_OPC_network](./logiBUS_AI_Raw_Publish_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_AI_Raw_Publish_OPC` liest einen Analog-Eingang (`logiBUS_AI_IDA`) als Rohwert (DWORD, 0-4095) und publiziert ihn per OPC-UA, **ohne jegliche VT-Interaktion**. Er ist für Sensoren gedacht, deren physischer Analog-Eingang auf einer Station ohne VT-Anschluss sitzt (z. B. Hangausgleich-Winkelsensor B14 auf APIXON STG3). Die Kalibrierung sowie VT-Taster und -Anzeigen laufen dann auf der Station mit VT-Anschluss und werden per Remote-Subscribe dieses Rohwerts gespeist (siehe [`logiBUS_AI_Calibrate_3P_Remote_IDA_OPC`](logiBUS_AI_Calibrate_3P_Remote_IDA_OPC.md)).

![logiBUS_AI_Raw_Publish_OPC](logiBUS_AI_Raw_Publish_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung; geht an `logiBUS_AI_IDA.INIT`

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierung abgeschlossen; kommt von `AD_PUBLISH_RAW.INITO`

### **Daten-Eingänge**

- **QI** (BOOL): Quality Input - TRUE aktiviert den Dienst (Default `TRUE`); geht an `logiBUS_AI_IDA.QI` und `AD_PUBLISH_RAW.QI`
- **Input** (`logiBUS::io::AI::logiBUS_AI_S`): Auswahl des Analog-Eingangs `AnalogInput_I1`..`I8` (Default `logiBUS_AI::Invalid`)
- **ID_RAW_WRITE** (WSTRING): Publish-Adresse des Rohwerts (DWORD 0-4095, `ACTION=PUBLISH`), lokal auf dieser Station

### **Daten-Ausgänge**

- **QO** (BOOL): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde (AND aus `logiBUS_AI_IDA.QO` und `AD_PUBLISH_RAW.QO`)

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Das Netzwerk besteht aus drei Bausteinen (zwei Funktionsbausteine plus die Verknüpfung `AND_QO`):

- `logiBUS_AI_IDA` (`logiBUS::io::AI::logiBUS_AI_IDA`, `QI` von außen): liest den gewählten Analog-Eingang (`Input` → `logiBUS_AI_IDA.Input`). Parametriert sind `AnalogInput_hysteresis = DWORD#50`, `TimeDelta = DWORD#250` und `TimeRateLimit = DWORD#100`.
- `AD_PUBLISH_RAW` (`adapter::net::AD_PUBLISH_1`, `QI` von außen): `logiBUS_AI_IDA.IN` → `AD_PUBLISH_RAW.IN`; publiziert den Rohwert an die Adresse `ID_RAW_WRITE` (`ID_RAW_WRITE` → `AD_PUBLISH_RAW.ID`).
- `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`): verknüpft `logiBUS_AI_IDA.QO` (`IN1`) und `AD_PUBLISH_RAW.QO` (`IN2`) zum Ausgang `QO`; `REQ` kommt von `AD_PUBLISH_RAW.INITO`.

Initialisierung: `INIT` → `logiBUS_AI_IDA.INIT`, `logiBUS_AI_IDA.INITO` → `AD_PUBLISH_RAW.INIT`, `AD_PUBLISH_RAW.INITO` → `INITO` (und → `AND_QO.REQ`).

## Technische Besonderheiten

- Rohwert-Hälfte von `logiBUS_AI_Calibrate_3P_IDA_OPC`: Die Kalibrierung findet nicht hier, sondern auf der Station mit VT-Anschluss statt
- Keine weiteren Ports außer `INIT`/`INITO`, `QI`/`QO` und den beiden Eingangsvariablen `Input` und `ID_RAW_WRITE`; keine Adapter
- Das Verhalten des Rohwert-Eingangs (Hysterese, `TimeDelta`, `TimeRateLimit`) wird über die Parameter von `logiBUS_AI_IDA` festgelegt

## Anwendungsszenarien

- Sensoren an Stationen ohne VT-Anschluss, deren Kalibrierung und Anzeige auf einer anderen Station laufen

## Fazit

`logiBUS_AI_Raw_Publish_OPC` stellt den Rohwert eines Analog-Eingangs per OPC-UA bereit und ist damit die Sensor-Seite einer stationsübergreifenden Kalibrierung.
