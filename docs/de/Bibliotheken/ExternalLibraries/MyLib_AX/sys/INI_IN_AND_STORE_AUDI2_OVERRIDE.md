# INI_IN_AND_STORE_AUDI2_OVERRIDE

![INI_IN_AND_STORE_AUDI2_OVERRIDE_network](./INI_IN_AND_STORE_AUDI2_OVERRIDE_network.svg)

* * * * * * * * * *

## Einleitung

`INI_IN_AND_STORE_AUDI2_OVERRIDE` ist wie [`INI_IN_AND_STORE_AUDI2`](./INI_IN_AND_STORE_AUDI2.md), jedoch mit Override-Möglichkeit: Ist der Socket `OVERRIDE_ACTIVE` (AX) dauerhaft TRUE, wird statt des Live-VT-Werts der Wert 1 (Deaktiviert) erzwungen - z. B. für gegenseitigen Optionsausschluss. Bei `OVERRIDE_ACTIVE`=FALSE (unbenutzt) ist das Verhalten identisch zu `INI_IN_AND_STORE_AUDI2`. Die Variante ohne `OVERRIDE_ACTIVE` ist als `INI_IN_AND_STORE_AUDI2` separat verfügbar (Umbenennung in Version 1.2).

Allgemeines Muster siehe [INI_IN_AND_STORE / NVS_IN_AND_STORE (gemeinsames Muster)](./INI-NVS-Speicherbausteine.md).

![INI_IN_AND_STORE_AUDI2_OVERRIDE](INI_IN_AND_STORE_AUDI2_OVERRIDE.svg)

## Schnittstelle

### Eingangsvariablen

- `KEY` (STRING): Schlüsselname in der INI-Datei.
- `SECTION` (STRING): Abschnittsname in der INI-Datei.
- `u16ObjId` (UINT, Default `ID_NULL`): ObjectID des VT-Eingabefelds.

### Adapter

- **Plug `VALUEO`** (`adapter::types::unidirectional::AUDI`): Wert für einen weiteren Consumer.
- **Socket `OVERRIDE_ACTIVE`** (`adapter::types::unidirectional::AX`): bei dauerhaft TRUE wird VAL=1 (Deaktiviert) erzwungen.

Der Baustein besitzt keine eigenen Ereignisanschlüsse.

## Funktionsweise

1. `ID` (`NumericValue_IDA`, `QI`=TRUE) liest das VT-Eingabefeld; `AD_TO_AUDI` wandelt in AUDI.
2. `SEL_Override` (`AUDI_AX_SEL_AUDI`) wählt: `IN0` = Live-VT-Wert, `IN1` = Override-Wert aus `LIT_Deaktiviert` (`initval_AUDI`, `INIT_VAL`=`UDINT#1`), Auswahl über `G` = `OVERRIDE_ACTIVE`.
3. `AUDI_TO_AUDI2` wandelt in den bidirektionalen AUDI2-Adapter für `INI_AUDI2` (`eclipse4diac::storage::INI_AUDI2`; `QI`=TRUE, `SETM`=TRUE, `DEFAULT_VALUE`=`UDINT#0`).
4. `AUDI_TO_AUDI2.AUDI_OUT` wird per `AUDI_SPLIT_2` verdoppelt: `OUT1` an `VALUEO`, `OUT2` an `Q_NumericValue_AUDI` (`u32NewValue`, VT-Anzeige).

## Technische Besonderheiten

- Der Override läuft über denselben `INI_AUDI2`/`Q_NumericValue_AUDI`-Pfad: `Q_NumericValue` erlaubt laut eigener Doku nur eine Instanz je ObjectID, eine zweite Schreibinstanz ist daher nicht möglich.
- Für reine Enum-/Ganzzahlwerte (z. B. Dropdowns 0-3), keine Dezimalstellen.
- `SETM`=TRUE: Der Boot-Wert lädt automatisch (INIT → INITO → GET → GETO); `SETM`=TRUE sorgt zusätzlich dafür, dass auch eine Live-Änderung sofort bestätigt/zurückgemeldet wird.

## Zusammenfassung

AUDI2-Speicherbaustein mit zuschaltbarem Override auf 1 (Deaktiviert), z. B. für gegenseitigen Optionsausschluss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
