# INI_IN_AND_STORE_AUDI2

![INI_IN_AND_STORE_AUDI2_network](./INI_IN_AND_STORE_AUDI2_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AUDI2` lädt und speichert `AUDI2`-Daten (reine Ganzzahl 0..N, ohne Skalierung/Nachkommastellen) persistent in einer INI-Datei und stellt zusätzlich einen Ausgang `VALUEO` (`AUDI`, plain) für einen weiteren Consumer bereit. Der Baustein ist analog zu [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md), arbeitet aber über den bidirektionalen `AUDI2`-Adapter statt `AR2`. Das allgemeine Muster beschreibt [INI_IN_AND_STORE / NVS_IN_AND_STORE](./INI-NVS-Speicherbausteine.md); eine Variante mit `AUDI` ist [`INI_IN_AND_STORE_AUDI`](./INI_IN_AND_STORE_AUDI.md).

![INI_IN_AND_STORE_AUDI2](INI_IN_AND_STORE_AUDI2.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst
- **KEY** (`STRING`): Schlüsselname in der INI-Datei
- **SECTION** (`STRING`): Abschnittsname in der INI-Datei
- **u16ObjId** (`UINT`): Object ID der VT-Zahlenvariable (Default `ID_NULL`)

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde

### **Adapter**

- **VALUEO** (Plug, `adapter::types::unidirectional::AUDI`): Wert für einen weiteren Consumer

## Verwendete Funktionsbausteine (FBs)

- **`ID`** (`isobus::UT::io::NumericValue::NumericValue_IDA`, `QI = TRUE`): liefert den vom Bediener eingegebenen VT-Wert
- **`AD_TO_AUDI`** (`adapter::conversion::unidirectional::AD_TO_AUDI`): wandelt den `AD`-Wert in `AUDI`
- **`AUDI_TO_AUDI2`** (`adapter::conversion::bidirectional::AUDI_TO_AUDI2`): wandelt `AUDI` in den bidirektionalen `AUDI2`-Adapter
- **`INI_AUDI2`** (`eclipse4diac::storage::INI_AUDI2`): `QI = TRUE`, `SETM = TRUE`, `DEFAULT_VALUE = UDINT#0`; lädt und speichert den Wert
- **`AUDI_SPLIT_2`** (`adapter::events::unidirectional::AUDI_SPLIT_2`): verdoppelt den Wert
- **`Q_NumericValue_AUDI`** (`isobus::UT::Q::Q_NumericValue_AUDI`): schreibt den Wert in die VT-Zahlenvariable

## Funktionsweise

- `ID.IN` → `AD_TO_AUDI.AD_IN` → `AUDI_TO_AUDI2.AUDI_IN`
- `AUDI_TO_AUDI2.AUDI2_BIDIR` → `INI_AUDI2.VAL` (Laden/Speichern)
- `AUDI_TO_AUDI2.AUDI_OUT` → `AUDI_SPLIT_2.IN`
- `AUDI_SPLIT_2.OUT1` → `VALUEO`; `AUDI_SPLIT_2.OUT2` → `Q_NumericValue_AUDI.u32NewValue` (VT-Anzeige)
- `KEY`, `SECTION` → `INI_AUDI2`; `u16ObjId` → `ID` und `Q_NumericValue_AUDI`
- `QI` → `Q_NumericValue_AUDI.QI`; `Q_NumericValue_AUDI.QO` → `QO`

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
