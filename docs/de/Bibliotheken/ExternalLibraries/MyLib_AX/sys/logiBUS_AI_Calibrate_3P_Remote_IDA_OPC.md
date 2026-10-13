# logiBUS_AI_Calibrate_3P_Remote_IDA_OPC

![logiBUS_AI_Calibrate_3P_Remote_IDA_OPC_network](./logiBUS_AI_Calibrate_3P_Remote_IDA_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_AI_Calibrate_3P_Remote_IDA_OPC` führt eine **3-Punkt-Kalibrierung** (`AR_CALIBRATE_3P_REF`) für einen Analog-Rohwert durch. Die Rohwerte `X_MIN`/`X_MID`/`X_MAX` und die Referenzwerte `MIN_REF`/`MID_REF`/`MAX_REF` werden unabhängig voneinander gespeichert und alle INI-persistiert. Die Ergebnisse erscheinen auf VT-Zahlenfeldern (Rohwert und kalibrierter Wert, je eigene CNumberVariable), drei VT-Tasten (`C_MIN`/`C_MID`/`C_MAX`) lösen die Kalibrierpunkte aus, und per OPC-UA werden der Rohwert (DWORD) und der kalibrierte Wert (REAL) generisch für einen Kanal bereitgestellt. Im Unterschied zu `logiBUS_AI_Calibrate_3P_IDA_OPC` kommt der Rohwert **per Remote-Subscribe** (`AD_SUBSCRIBE_1`) statt von einem lokalen `logiBUS_AI_IDA`; so lassen sich Sensoren auf einer Station ohne VT-Anschluss kalibrieren (z. B. Hangausgleich-Winkelsensor B14 auf STG3), während Kalibrierung und VT auf der Station mit VT-Anschluss laufen. Der Rohwert wird dort per [`logiBUS_AI_Raw_Publish_OPC`](logiBUS_AI_Raw_Publish_OPC.md) publiziert.

![logiBUS_AI_Calibrate_3P_Remote_IDA_OPC](logiBUS_AI_Calibrate_3P_Remote_IDA_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

- **u16ObjId_RAWVAR** (UINT): Object ID der CNumberVariable für den Rohwert (0-4095, unkalibriert, reine Diagnose-Anzeige)
- **stObjCal** (`NumericObjectPool_S`): Object ID + Skalierung/Offset/Dezimalstellen der CNumberVariable für den kalibrierten Wert (eigene, unabhängige Variable)
- **u16ObjId_C_MIN**, **u16ObjId_C_MID**, **u16ObjId_C_MAX** (UINT): Object IDs der Tasten `C_MIN`, `C_MID`, `C_MAX` (Position anfahren, dann drücken)
- **ID_RAW_REMOTE_READ** (WSTRING): Remote-Subscribe-Adresse des Rohwerts (DWORD 0-4095, `ACTION=SUBSCRIBE`); Gegenstück zu `ID_RAW_WRITE` auf der Sensor-Station
- **sSection** (STRING): INI-Sektionsname für die Persistenz von `X_MIN`/`X_MID`/`X_MAX`/`MIN_REF`/`MID_REF`/`MAX_REF`; pro Kanal eindeutig, sonst kollidieren die Kanäle in `settings.ini`
- **ID_CAL_WRITE** (WSTRING): Rückmelde-Adresse des kalibrierten Werts (REAL, `ACTION=PUBLISH`)
- **ID_MIN_READ**, **ID_MID_READ**, **ID_MAX_READ** (WSTRING): Remote-Adressen (BOOL, `ACTION=SUBSCRIBE`) für `C_MIN`/`C_MID`/`C_MAX`; jede Flanke (der Web-Client schreibt bei jedem Klick den invertierten letzten Wert) löst den Kalibrierpunkt wie ein Tastendruck aus
- **ID_MID_TRIGGER_METHOD** (WSTRING): lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argument- und rückgabewertlosen `C_MID`-Trigger; wird von einem lokalen `CLIENT_0` (z. B. `Fahrwerk_Mittelpunkt_Automatik.SUB`) per `CALL_METHOD` aufgerufen und löst `C_MID` wie `Button_C_MID`/`ID_MID_READ` aus
- **stObjMinRef**, **stObjMidRef**, **stObjMaxRef** (`NumericObjectPool_S`): Object ID + Skalierung/Offset/Dezimalstellen der Referenzwerte `MIN_REF`/`MID_REF`/`MAX_REF` (VT-Eingabefeld + OPC-UA)
- **ID_MINREF_READ**, **ID_MIDREF_READ**, **ID_MAXREF_READ** (WSTRING): Remote-Subscribe-Adressen der Referenzwerte (REAL, `ACTION=SUBSCRIBE`)
- **ID_MINREF_WRITE**, **ID_MIDREF_WRITE**, **ID_MAXREF_WRITE** (WSTRING): Remote-Echo-Adressen der Referenzwerte (REAL, `ACTION=PUBLISH`)

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **CAL_OUT** (Plug, `AR`): optional; kalibrierter Wert (`CALIBRATE.Y`) für externe Konsumenten, statt eines eigenen lokalen OPC-UA-Self-Subscribe auf `ID_CAL_WRITE`
- **MIDREF_OUT** (Plug, `AR`): optional; live-`MID_REF`-Referenzwert (`MID_REF_LIT.VALUE_OUT`, einfacher AR-Spiegel des bidirektionalen `VALUEO`) für externe Konsumenten
- **MAXREF_OUT** (Plug, `AR`): optional; live-`MAX_REF`-Referenzwert (`MAX_REF_LIT.VALUE_OUT`) für externe Konsumenten

## Funktionsweise

**Rohwert**: `AD_SUBSCRIBE_RAW` (`adapter::net::AD_SUBSCRIBE_1`, `QI = TRUE`, Adresse `ID_RAW_REMOTE_READ`) → `AD_TO_AUDI` → `AUDI_SPLIT_2`. `OUT1` → `Q_NumericValue_RAW.u32NewValue` (`Q_NumericValue_AUDI`, Rohwert-Anzeige auf `u16ObjId_RAWVAR`); `OUT2` → `AUDI_TO_AR` → `CALIBRATE.X`.

**Kalibrierung**: `CALIBRATE` (`adapter::Engineering::measurements::AR_CALIBRATE_3P_REF`, `QI = TRUE`, `xClipping = FALSE`) bildet den Rohwert `X` über die drei Punkte auf `Y` ab. `Y_MIN`/`Y_MID`/`Y_MAX` kommen von `MIN_REF_LIT`/`MID_REF_LIT`/`MAX_REF_LIT` (je [`MyLib::sys::NumericValue_TO_AR2_OPC`](NumericValue_TO_AR2_OPC.md); VT-Eingabefeld und OPC-UA, letzter Schreiber gewinnt).

**Ergebnis**: `CALIBRATE.Y` → `AR_SPLIT_2` (`AR_SPLIT_3`): `OUT1` → `AR_PUBLISH_CAL` (`AR_PUBLISH_1`, Adresse `ID_CAL_WRITE`), `OUT2` → `Q_NumericValue_CAL.rPhys` (`Q_NumericValue_PHYSA`, Anzeige mit `stObjCal`), `OUT3` → `CAL_OUT`. `MID_REF_LIT.VALUE_OUT` → `MIDREF_OUT` und `MAX_REF_LIT.VALUE_OUT` → `MAXREF_OUT`.

**Trigger**: Jeder Kalibrierpunkt hat einen eigenen Event-Merge. `C_MIN`: `Button_C_MIN.IND` (`Button_IE`, `BT_PRESSED_LATCHED`), `AX_RF_TRIG_MIN.ER` und `.EF` (beide Flanken des Remote-Subscribe `AX_SUBSCRIBE_MIN`, Adresse `ID_MIN_READ`) → `E_MERGE_MIN` (`E_MERGE_3`) → `CALIBRATE.C_MIN`. `C_MAX` analog über `Button_C_MAX`, `AX_SUBSCRIBE_MAX`, `AX_RF_TRIG_MAX`, `E_MERGE_MAX` (`E_MERGE_3`). `C_MID` zusätzlich mit `SERVER_MID_TRIGGER` (`iec61499::net::SERVER_0`, Methode `ID_MID_TRIGGER_METHOD`) → `E_MERGE_MID` (`E_MERGE_4`) → `CALIBRATE.C_MID`.

**Persistenz**: `CALIBRATE.X_MIN`/`X_MID`/`X_MAX` und `MIN_REF`/`MID_REF`/`MAX_REF` gehen an je eine `INI_AR2`-Instanz (`INI_X_MIN`, `INI_X_MID`, `INI_X_MAX`, `INI_MIN_REF`, `INI_MID_REF`, `INI_MAX_REF`; `SETM = TRUE`, `SECTION` aus `sSection`, `KEY` = Name). Die Defaults sind `X_MIN`/`X_MID`/`X_MAX` = `REAL#0.0` sowie `MIN_REF = REAL#-100.0`, `MID_REF = REAL#50.0`, `MAX_REF = REAL#100.0`.

**Initialisierung**: `AD_SUBSCRIBE_RAW.INITO` → `AR_PUBLISH_CAL.INIT` → `AX_SUBSCRIBE_MIN.INIT` → `AX_SUBSCRIBE_MID.INIT` → `AX_SUBSCRIBE_MAX.INIT` → `SERVER_MID_TRIGGER.INIT`.

## Technische Besonderheiten

- Die Defaults von `X_MID`/`X_MAX` stehen bewusst auf `0.0`: Mit `0.5`/`1.0` latchten die Kalibriert-Flags `xMidCal`/`xMaxCal` von `AR_CALIBRATE_3P_REF` bei jedem Neustart sofort auf TRUE, obwohl MID/MAX nie kalibriert wurden (Fix in Version 1.6)
- `CALIBRATE` hat ein `INIT`/`QI`/`QO`-Enable-Muster; ohne `QI = TRUE` bleibt der Baustein im Zustand START hängen und `REQ` läuft nie (Version 1.5)
- `MIDREF_OUT`/`MAXREF_OUT` werden aus `VALUE_OUT` gespeist, nicht aus dem bidirektionalen `VALUEO`: ein `AR_SPLIT_2` direkt auf einen AR2-Plug wäre ein Typfehler (Version 1.2)
- Die optionalen Plugs `CAL_OUT`/`MIDREF_OUT`/`MAXREF_OUT` erlauben externen Konsumenten, den bereits kalibrierten Wert per Adapter zu lesen, statt denselben OPC-UA-Knoten lokal nochmals zu abonnieren (FORTE: 'Cannot add another one'); unverdrahtet lassen, wenn nicht gebraucht (z. B. Hangausgleich-Kanal)
- Die Merge-Ketten für `C_MIN`/`C_MID`/`C_MAX` sind je ein einziger `E_MERGE_3`/`E_MERGE_4` statt mehrerer `E_MERGE_2` (ODER ist assoziativ, funktional identisch); `C_MIN`/`C_MID`/`C_MAX` von `CALIBRATE` sind reine EventInputs
- Remote-Trigger lösen bei **jeder** Flanke aus (steigend und fallend), da der Web-Client bei jedem Klick den invertierten letzten Wert schreibt

## Anwendungsszenarien

- Kalibrierung eines Winkel- oder Positionssensors (z. B. Hangausgleich), dessen Analog-Eingang auf einer anderen Station sitzt als das VT
- Dreipunktkalibrierung mit Min-, Mitte- und Max-Stellung samt einstellbaren Referenzwerten und persistierten Rohwerten

## ⚖️ Vergleich mit ähnlichen Bausteinen

`logiBUS_AI_Calibrate_3P_IDA_OPC` liest den Rohwert lokal von einem `logiBUS_AI_IDA`; diese Remote-Variante ersetzt dessen `logiBUS_AI_IDA` + `AD_SPLIT_2` + `AD_PUBLISH_RAW` durch ein `AD_SUBSCRIBE_1`. Die Kette von Kalibrierung und VT-Anbindung ist unverändert. Die Sensor-Seite ist [`logiBUS_AI_Raw_Publish_OPC`](logiBUS_AI_Raw_Publish_OPC.md).

## Fazit

`logiBUS_AI_Calibrate_3P_Remote_IDA_OPC` kalibriert einen remote abonnierten Analog-Rohwert über drei Punkte, persistiert alle Kalibrierdaten, zeigt Rohwert und kalibrierten Wert am VT und stellt sie per OPC-UA bereit.
