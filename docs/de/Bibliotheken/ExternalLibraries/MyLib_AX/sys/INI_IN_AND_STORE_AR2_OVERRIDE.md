# INI_IN_AND_STORE_AR2_OVERRIDE

![INI_IN_AND_STORE_AR2_OVERRIDE_network](./INI_IN_AND_STORE_AR2_OVERRIDE_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AR2_OVERRIDE` ist wie [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md), jedoch mit Override-Möglichkeit: Ist der Socket `OVERRIDE_ACTIVE` (AX) dauerhaft TRUE, wird statt des Live-VT-Werts ein konfigurierter Wert erzwungen (Parameter `INIT_VAL` an `LIT_Override`, Default `0.0`). Bei `OVERRIDE_ACTIVE`=FALSE (unbenutzt) ist das Verhalten identisch zu `INI_IN_AND_STORE_AR2`.

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![INI_IN_AND_STORE_AR2_OVERRIDE](INI_IN_AND_STORE_AR2_OVERRIDE.svg)

## Schnittstelle

### Eingangsvariablen

- `QI` (BOOL, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst; wird an `NumericValue_PHYSA`, `INI_AR2` und `Q_NumericValue_PHYSA` durchgereicht.
- `KEY` (STRING): Schlüsselname in der INI-Datei.
- `SECTION` (STRING): Abschnittsname in der INI-Datei.
- `stObj` (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): VT-Objekt mit `u16ObjId`, `r32Scale`, `i32Offset`, `u8Decimals` (Default `u16ObjId := ID_NULL`, `r32Scale := 1.0`, `i32Offset := 0`, `u8Decimals := 0`).

### Ausgangsvariablen

- `QO` (BOOL): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde; `AND` der `QO`-Ausgänge von `Q_NumericValue_PHYSA`, `NumericValue_PHYSA` und `INI_AR2` (über `AND_QO`).

### Adapter

- **Plug `VALUEO`** (`adapter::types::unidirectional::AR`): Wert für einen weiteren Consumer.
- **Socket `OVERRIDE_ACTIVE`** (`adapter::types::unidirectional::AX`): bei dauerhaft TRUE wird der Override-Wert erzwungen.

### Ereignisse

- `INIT` (EInit, Eingang): Initialisierungsanforderung; startet die Kette `NumericValue_PHYSA` → `INI_AR2` → `Q_NumericValue_PHYSA`.
- `INITO` (EInit, Ausgang): Initialisierungsbestätigung; kommt von `Q_NumericValue_PHYSA.INITO`.

## Funktionsweise

1. `NumericValue_PHYSA` liest das VT-Eingabefeld als physikalischen Wert (`rPhys`).
2. `SEL_Override` (`AR_AX_SEL_AR`) wählt: `IN0` = Live-VT-Wert, `IN1` = Override-Wert aus `LIT_Override` (`initval_AR`, `INIT_VAL`=`REAL#0.0`), Auswahl über `G` = `OVERRIDE_ACTIVE`.
3. `AR_TO_AR2` wandelt in den bidirektionalen AR2-Adapter für `INI_AR2` (`eclipse4diac::storage::INI_AR2`; `SETM`=TRUE, `DEFAULT_VALUE`=`REAL#0.0`; `KEY`/`SECTION` von außen).
4. `AR_TO_AR2.AR_OUT` wird per `AR_SPLIT_2` verdoppelt: `OUT1` an `VALUEO`, `OUT2` an `Q_NumericValue_PHYSA` (VT-Anzeige, `stObj`).

`QI` wird an `NumericValue_PHYSA.QI`, `INI_AR2.QI` und `Q_NumericValue_PHYSA.QI` durchgereicht (kein fester `QI`=TRUE mehr). Die Initialisierung läuft als Kette `INIT` → `NumericValue_PHYSA.INIT`, `NumericValue_PHYSA.INITO` → `INI_AR2.INIT`, `INI_AR2.INITO` → `Q_NumericValue_PHYSA.INIT`, `Q_NumericValue_PHYSA.INITO` → `INITO` und → `AND_QO.REQ`. `QO` ist das `AND` (`AND_QO`, `AND_BOOL_3`) der `QO`-Ausgänge von `Q_NumericValue_PHYSA`, `NumericValue_PHYSA` und `INI_AR2`.

## Technische Besonderheiten

- Im Unterschied zur AUDI2-Variante ([`INI_IN_AND_STORE_AUDI2_OVERRIDE`](./INI_IN_AND_STORE_AUDI2_OVERRIDE.md), Enum/Dropdown, 1=Deaktiviert) ist AR ein kontinuierlicher physikalischer Wert: Der Override-Wert hat keine feste Bedeutung, sondern ist über `INIT_VAL` an `LIT_Override` frei konfigurierbar.
- `SETM`=TRUE: Der Boot-Wert lädt automatisch (INIT → INITO → GET → GETO, unabhängig von `SETM`); `SETM`=TRUE sorgt zusätzlich dafür, dass auch eine Live-Änderung im laufenden Betrieb sofort bestätigt/zurückgemeldet wird (z. B. für die Weiterverwendung über `VALUEO`).

## Zusammenfassung

AR2-Speicherbaustein mit zuschaltbarem, konfigurierbarem Override-Wert.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
