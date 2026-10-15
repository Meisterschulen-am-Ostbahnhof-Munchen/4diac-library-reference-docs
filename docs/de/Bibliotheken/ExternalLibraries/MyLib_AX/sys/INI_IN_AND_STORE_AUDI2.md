# INI_IN_AND_STORE_AUDI2

![INI_IN_AND_STORE_AUDI2_network](./INI_IN_AND_STORE_AUDI2_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AUDI2` lädt und speichert `AUDI2`-Daten (reine Ganzzahl 0..N, ohne Skalierung/Nachkommastellen) persistent in einer INI-Datei und stellt zusätzlich einen Ausgang `VALUEO` (`AUDI`, plain) für einen weiteren Consumer bereit. Der Baustein ist analog zu [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md), arbeitet aber über den bidirektionalen `AUDI2`-Adapter statt `AR2`. Das allgemeine Muster beschreibt [INI_IN_AND_STORE / NVS_IN_AND_STORE](./INI-NVS-Speicherbausteine.md); eine Variante mit `AUDI` ist [`INI_IN_AND_STORE_AUDI`](./INI_IN_AND_STORE_AUDI.md).

![INI_IN_AND_STORE_AUDI2](INI_IN_AND_STORE_AUDI2.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst
- **KEY** (`STRING`): Schlüsselname in der INI-Datei
- **SECTION** (`STRING`): Abschnittsname in der INI-Datei
- **u16ObjId** (`UINT`): Object ID der VT-Zahlenvariable (Default `ID_NULL`)

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde (UND aus `Q_NumericValue_AUDI.QO`, `ID.QO` und `INI_AUDI2.QO`)

### **Adapter**

- **VALUEO** (Plug, `adapter::types::unidirectional::AUDI`): Wert für einen weiteren Consumer

## Verwendete Funktionsbausteine (FBs)

- **`ID`** (`isobus::UT::io::NumericValue::NumericValue_IDA`): liefert den vom Bediener eingegebenen VT-Wert
- **`AD_TO_AUDI`** (`adapter::conversion::unidirectional::AD_TO_AUDI`): wandelt den `AD`-Wert in `AUDI`
- **`AUDI_TO_AUDI2`** (`adapter::conversion::bidirectional::AUDI_TO_AUDI2`): wandelt `AUDI` in den bidirektionalen `AUDI2`-Adapter
- **`INI_AUDI2`** (`eclipse4diac::storage::INI_AUDI2`): `SETM = TRUE`, `DEFAULT_VALUE = UDINT#0`; lädt und speichert den Wert
- **`AUDI_SPLIT_2`** (`adapter::events::unidirectional::AUDI_SPLIT_2`): verdoppelt den Wert
- **`Q_NumericValue_AUDI`** (`isobus::UT::Q::Q_NumericValue_AUDI`): schreibt den Wert in die VT-Zahlenvariable
- **`AND_QO`** (`iec61131::booleanOperators::AND_BOOL_3`): verknüpft die `QO`-Ausgänge von `Q_NumericValue_AUDI`, `ID` und `INI_AUDI2`

## Funktionsweise

- `ID.IN` → `AD_TO_AUDI.AD_IN` → `AUDI_TO_AUDI2.AUDI_IN`
- `AUDI_TO_AUDI2.AUDI2_BIDIR` → `INI_AUDI2.VAL` (Laden/Speichern)
- `AUDI_TO_AUDI2.AUDI_OUT` → `AUDI_SPLIT_2.IN`
- `AUDI_SPLIT_2.OUT1` → `VALUEO`; `AUDI_SPLIT_2.OUT2` → `Q_NumericValue_AUDI.u32NewValue` (VT-Anzeige)
- `KEY`, `SECTION` → `INI_AUDI2`; `u16ObjId` → `ID` und `Q_NumericValue_AUDI`
- `INIT` → `ID.INIT`; `ID.INITO` → `INI_AUDI2.INIT`; `INI_AUDI2.INITO` → `Q_NumericValue_AUDI.INIT`; `Q_NumericValue_AUDI.INITO` → `INITO` (und → `AND_QO.REQ`)
- `QI` → `Q_NumericValue_AUDI.QI`, `ID.QI` und `INI_AUDI2.QI`
- `Q_NumericValue_AUDI.QO` → `AND_QO.IN1`; `ID.QO` → `AND_QO.IN2`; `INI_AUDI2.QO` → `AND_QO.IN3`; `AND_QO.OUT` → `QO`

## Technische Besonderheiten

- **`SETM = TRUE`:** Der Boot-Wert wird ohnehin automatisch geladen (`INIT` → `INITO` → `GET` → `GETO`, unabhängig von `SETM`). `SETM = TRUE` sorgt dafür, dass auch eine Live-Änderung im laufenden Betrieb sofort bestätigt bzw. zurückgemeldet wird; wichtig, wenn der frische Wert im selben Lauf weiterverwendet werden soll (hier über `VALUEO`).
- `INI_AUDI2` besitzt keinen eigenen Ausgangs-Plug, daher wird der Ausgang per `AUDI_SPLIT_2` verdoppelt
- Reine Ganzzahl (`UDINT`) ohne Skalierung

## Anwendungsszenarien

- Persistent gespeicherte Ganzzahl-Einstellung (z. B. Zähler, Auswahlindex), die am VT eingegeben und weiterverwendet wird

## Zusammenfassung

`INI_IN_AND_STORE_AUDI2` speichert einen VT-Ganzzahlwert in einer INI-Datei (über `AUDI2`) und gibt ihn zusätzlich als `AUDI` aus.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
