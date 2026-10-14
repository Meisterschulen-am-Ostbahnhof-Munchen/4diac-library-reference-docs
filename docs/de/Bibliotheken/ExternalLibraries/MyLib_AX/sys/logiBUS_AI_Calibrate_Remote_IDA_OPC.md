# logiBUS_AI_Calibrate_Remote_IDA_OPC

![logiBUS_AI_Calibrate_Remote_IDA_OPC_network](./logiBUS_AI_Calibrate_Remote_IDA_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_AI_Calibrate_Remote_IDA_OPC` ist eine 2-Punkt-Kalibrierung (`AR_CALIBRATE_2P_REF`, unabhängig gespeicherte Rohwerte `X_LOW`/`X_HIGH` und Referenzwerte `LOW_REF`/`HIGH_REF`, alle INI-persistiert) mit VT-Zahlenfeldern (Rohwert und kalibrierter Wert, je eigene `CNumberVariable`), zwei VT-Tasten (CO/CS) und OPC-UA (Rohwert `DWORD`, kalibrierter Wert `REAL`, generisch, ein Kanal). Es entspricht [`logiBUS_AI_Calibrate_2P_IDA_OPC`](./logiBUS_AI_Calibrate_2P_IDA_OPC.md), aber der Rohwert kommt per **Remote-Subscribe** (`AD_SUBSCRIBE_1`) statt von einem lokalen `logiBUS_AI_IDA`. Das ist für Sensoren gedacht, deren physischer Analogeingang auf einer Station **ohne VT-Anschluss** sitzt (z. B. Schnitthöhen-Winkelsensor B13 auf APIXON STG3, während Kalibrierung und VT auf der Station mit VT-Anschluss laufen). Der Rohwert wird dort per [`logiBUS_AI_Raw_Publish_OPC`](./logiBUS_AI_Raw_Publish_OPC.md) publiziert.

![logiBUS_AI_Calibrate_Remote_IDA_OPC](logiBUS_AI_Calibrate_Remote_IDA_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialization Request

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialization Confirm

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input, TRUE aktiviert den Dienst
- **u16ObjId_RAWVAR** (`UINT`): Object ID `CNumberVariable` (Rohwert 0..4095, unkalibriert, reine Diagnose-Anzeige), Default `ID_NULL`
- **stObjCal** (`NumericObjectPool_S`): Object ID plus Scale/Offset/Decimals der `CNumberVariable` für den kalibrierten Wert (eigene, unabhängige Variable)
- **u16ObjId_CO** (`UINT`): Object ID Taste "CO" (Kalibrier-Nullpunkt: Referenz 1 anlegen, dann drücken)
- **u16ObjId_CS** (`UINT`): Object ID Taste "CS" (Kalibrier-Spanne: Referenz 2 anlegen, dann drücken; erst nach CO möglich)
- **ID_RAW_REMOTE_READ** (`WSTRING`): Remote-Subscribe-Adresse des Rohwerts (`DWORD` 0..4095, `ACTION=SUBSCRIBE`), Gegenstück zu `ID_RAW_WRITE` auf der Sensor-Station
- **sSection** (`STRING`): INI-Sektionsname für die Persistenz von `X_LOW`/`X_HIGH`/`LOW_REF`/`HIGH_REF`; pro Kanal eindeutig, sonst kollidieren die Kanäle in `settings.ini`
- **ID_CAL_WRITE** (`WSTRING`): Rückmelde-Adresse des kalibrierten Werts (`REAL`, `ACTION=PUBLISH`)
- **ID_CO_READ** (`WSTRING`): Remote-CO-Adresse (`BOOL`, `ACTION=SUBSCRIBE`): togglet den Kalibrier-Nullpunkt-Trigger wie ein Tastendruck; der Web-Client schreibt bei jedem Klick den invertierten letzten Wert, jede Flanke (steigend oder fallend) löst CO aus
- **ID_CS_READ** (`WSTRING`): Remote-CS-Adresse (`BOOL`, `ACTION=SUBSCRIBE`): togglet den Kalibrier-Spanne-Trigger, siehe `ID_CO_READ`
- **stObjZero** (`NumericObjectPool_S`): `CNumberVariable` des ZERO-Referenzwerts (VT-Eingabefeld + OPC-UA)
- **stObjSpan** (`NumericObjectPool_S`): `CNumberVariable` des SPAN-Referenzwerts (VT-Eingabefeld + OPC-UA)
- **ID_ZERO_READ** (`WSTRING`): Remote-Subscribe-Adresse ZERO-Referenzwert (`REAL`, `ACTION=SUBSCRIBE`)
- **ID_ZERO_WRITE** (`WSTRING`): Remote-Echo-Adresse ZERO-Referenzwert (`REAL`, `ACTION=PUBLISH`)
- **ID_SPAN_READ** (`WSTRING`): Remote-Subscribe-Adresse SPAN-Referenzwert (`REAL`, `ACTION=SUBSCRIBE`)
- **ID_SPAN_WRITE** (`WSTRING`): Remote-Echo-Adresse SPAN-Referenzwert (`REAL`, `ACTION=PUBLISH`)

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output, UND-Verknüpfung (`AND_QO`, ein einzelner `AND_BOOL_15`) der `QO`-Ausgänge aller 15 inneren Bausteine mit `QO`: `Q_NumericValue_RAW`, `Q_NumericValue_CAL`, `Y_OFFSET_LIT`, `Y_SCALE_LIT`, `CALIBRATE`, `INI_X_LOW`, `INI_X_HIGH`, `INI_LOW_REF`, `INI_HIGH_REF`, `AD_SUBSCRIBE_RAW`, `AR_PUBLISH_CAL`, `AX_SUBSCRIBE_CO`, `AX_SUBSCRIBE_CS`, `Button_CO`, `Button_CS`

### **Adapter**

Keine.

## Verwendete Funktionsbausteine (FBs)

- **`AD_SUBSCRIBE_RAW`** (`adapter::net::AD_SUBSCRIBE_1`): abonniert den Remote-Rohwert
- **`AD_TO_AUDI`**, **`AUDI_SPLIT_2`**, **`AUDI_TO_AR`**: Umwandlung und Aufteilung des Rohwerts
- **`Q_NumericValue_RAW`** (`isobus::UT::Q::Q_NumericValue_AUDI`): VT-Rohwertanzeige
- **`CALIBRATE`** (`adapter::Engineering::measurements::AR_CALIBRATE_2P_REF`, `xClipping = FALSE`): 2-Punkt-Kalibrierung
- **`Y_OFFSET_LIT`**, **`Y_SCALE_LIT`** (SubApp `MyLib::sys::NumericValue_TO_AR2_OPC`, [NumericValue_TO_AR2_OPC](./NumericValue_TO_AR2_OPC.md)): ZERO-/SPAN-Referenzwerte als `AR2`
- **`Button_CO`**, **`Button_CS`** (`isobus::UT::io::Button::Button_IE`, `InputEvent = BT_PRESSED_LATCHED`): VT-Tasten
- **`AX_SUBSCRIBE_CO`**, **`AX_SUBSCRIBE_CS`** (`adapter::net::AX_SUBSCRIBE_1`): Remote-Trigger
- **`AX_RF_TRIG_CO`**, **`AX_RF_TRIG_CS`** (`adapter::events::unidirectional::AX_RF_TRIG`): erkennen steigende und fallende Flanke
- **`E_MERGE_CO`**, **`E_MERGE_CS`** (`iec61499::events::E_MERGE_3`): führen Taste, steigende und fallende Flanke zusammen
- **`INI_X_LOW`**, **`INI_X_HIGH`**, **`INI_LOW_REF`**, **`INI_HIGH_REF`** (`eclipse4diac::storage::INI_AR2`, `SETM = TRUE`): Persistenz; Schlüssel `X_LOW`/`X_HIGH`/`LOW_REF`/`HIGH_REF`, Defaults `0.0`/`0.0`/`0.0`/`100.0`
- **`AND_QO`** (`iec61131::booleanOperators::AND_BOOL_15`, seit v1.5): ersetzt die frühere Kaskade `AND_QO_A` (`AND_BOOL_4`) + `AND_QO_B` (`AND_BOOL_10`) + `AND_QO` (`AND_BOOL_3`) durch einen einzelnen Baustein für alle 15 `QO`-Quellen
- **`AR_SPLIT_2`** (`adapter::events::unidirectional::AR_SPLIT_2`), **`AR_PUBLISH_CAL`** (`adapter::net::AR_PUBLISH_1`), **`Q_NumericValue_CAL`** (`isobus::UT::Q::Q_NumericValue_PHYSA`): Ausgabe des kalibrierten Werts

## Funktionsweise

- Rohwertpfad: `AD_SUBSCRIBE_RAW.OUT` → `AD_TO_AUDI.AD_IN` → `AUDI_SPLIT_2.IN`; `AUDI_SPLIT_2.OUT1` → `Q_NumericValue_RAW.u32NewValue` (VT-Anzeige); `AUDI_SPLIT_2.OUT2` → `AUDI_TO_AR.AUDI_IN` → `CALIBRATE.X`
- Persistenz: `CALIBRATE.X_LOW`/`X_HIGH`/`LOW_REF`/`HIGH_REF` ↔ `INI_X_LOW`/`INI_X_HIGH`/`INI_LOW_REF`/`INI_HIGH_REF.VAL`; `sSection` → `SECTION` aller vier
- Referenzen: `Y_OFFSET_LIT.VALUEO` → `CALIBRATE.Y_LOW`; `Y_SCALE_LIT.VALUEO` → `CALIBRATE.Y_HIGH`
- Kalibrierter Wert: `CALIBRATE.Y` → `AR_SPLIT_2.IN`; `OUT1` → `AR_PUBLISH_CAL.IN`; `OUT2` → `Q_NumericValue_CAL.rPhys`
- CO-Trigger: `Button_CO.IND` → `E_MERGE_CO.EI1`; `AX_SUBSCRIBE_CO.OUT` → `AX_RF_TRIG_CO.QI`; `AX_RF_TRIG_CO.ER` → `E_MERGE_CO.EI2`, `AX_RF_TRIG_CO.EF` → `E_MERGE_CO.EI3`; `E_MERGE_CO.EO` → `CALIBRATE.C_LOW`
- CS-Trigger: analog mit `Button_CS`, `AX_SUBSCRIBE_CS`, `AX_RF_TRIG_CS`, `E_MERGE_CS` → `CALIBRATE.C_HIGH`
- Initialisierungskette: `INIT` → `Q_NumericValue_RAW.INIT` → `Q_NumericValue_CAL.INIT` → `Y_OFFSET_LIT.INIT` → `Y_SCALE_LIT.INIT` → `CALIBRATE.INIT` → `INI_X_LOW.INIT` → `INI_X_HIGH.INIT` → `INI_LOW_REF.INIT` → `INI_HIGH_REF.INIT` → `AD_SUBSCRIBE_RAW.INIT` → `AR_PUBLISH_CAL.INIT` → `AX_SUBSCRIBE_CO.INIT` → `AX_SUBSCRIBE_CS.INIT` → `Button_CO.INIT` → `Button_CS.INIT`; `Button_CS.INITO` → `INITO` und → `AND_QO.REQ`. Zusätzlich führt `E_MERGE_QO` (`E_MERGE_3`) `Q_NumericValue_CAL.INITO` sowie die `CNF`-Ereignisse von `Q_NumericValue_RAW` und `Q_NumericValue_CAL` zusammen; sein Ausgang `EO` triggert `AND_QO.REQ`, sodass `QO` bei jedem neuen Wert neu ausgewertet wird (ab v1.6; vorher war `EO` nicht angeschlossen). `QI` wird an alle inneren Bausteine verteilt; ihre `QO` laufen in der Reihenfolge der Kette auf `AND_QO.IN1` bis `AND_QO.IN15`, `AND_QO.OUT` → `QO`. (Die bisherige Teilkette `AD_SUBSCRIBE_RAW.INITO` → `AR_PUBLISH_CAL.INIT` → `AX_SUBSCRIBE_CO.INIT` → `AX_SUBSCRIBE_CS.INIT` ist Teil dieser Kette.)

## Technische Besonderheiten

- **Rohwert per Remote-Subscribe:** `logiBUS_AI_IDA`, `AD_SPLIT_2` und `AD_PUBLISH_RAW` der lokalen Variante sind durch `AD_SUBSCRIBE_1` ersetzt, der Rest der Kalibrier- und VT-Kette ist unverändert.
- **CO/CS ohne AX-Sockets:** Seit v1.1 sind `C_LOW`/`C_HIGH` von `AR_CALIBRATE_2P_REF` reine Ereigniseingänge; die `E_MERGE_3`-Bausteine verbinden direkt per Ereignisverbindung (v1.2: ein `E_MERGE_3` statt zweier `E_MERGE_2` in Reihe, funktional identisch).
- **`QI` an `CALIBRATE`:** Seit v1.3 hat `AR_CALIBRATE_2P_REF` ein `INIT`/`QI`/`QO`-Enable-Muster; ohne `QI = TRUE` bliebe der Baustein im Zustand `START` und `REQ` liefe nie. Das `QI` aller inneren Bausteine kommt jetzt vom SubApp-Eingang `QI` (Vorgabe `TRUE`) statt aus festen Parametern.
- **Bugfix v1.4 (Code-Review PR #260):** `INI_X_HIGH.DEFAULT_VALUE` stand auf `REAL#1.0` statt `0.0`; dadurch latchte das Kalibriert-Flag `xHighCal` bei jedem Boot sofort auf TRUE, obwohl HIGH nie kalibriert wurde. Betrifft Schnitthöhe, Öltemperatur und Druck.
- **Web-Trigger:** CO/CS werden von jeder Flanke (steigend oder fallend) ausgelöst, da der Web-Client bei jedem Klick den invertierten letzten Wert schreibt.
- Dieser Typ wird durch `logiBUS_AI_Calibrated_Subscribe_VT_OPC` (lokale Analog-Verarbeitung) abgelöst; die Anpassungen halten die Datei kompilierbar.

## Anwendungsszenarien

- Kalibrierung eines Sensors, dessen Analogeingang auf einer Station ohne VT-Anschluss sitzt, mit VT-Bedienung auf einer anderen Station

## ⚖️ Vergleich mit ähnlichen Bausteinen

- [`logiBUS_AI_Calibrate_2P_IDA_OPC`](./logiBUS_AI_Calibrate_2P_IDA_OPC.md): Rohwert von einem lokalen `logiBUS_AI_IDA`
- [`logiBUS_AI_Calibrate_3P_Remote_IDA_OPC`](./logiBUS_AI_Calibrate_3P_Remote_IDA_OPC.md): Drei-Punkt-Variante mit Remote-Rohwert

## Zusammenfassung

`logiBUS_AI_Calibrate_Remote_IDA_OPC` kalibriert einen remote publizierten Analog-Rohwert in zwei Punkten, zeigt Roh- und kalibrierten Wert am VT an und veröffentlicht den kalibrierten Wert per OPC-UA.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
