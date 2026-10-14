# NumericValue_TO_AUI2_AND_STORE_PUBLISH

![NumericValue_TO_AUI2_AND_STORE_PUBLISH_network](./NumericValue_TO_AUI2_AND_STORE_PUBLISH_network.svg)

* * * * * * * * * *

## Einleitung

`NumericValue_TO_AUI2_AND_STORE_PUBLISH` ist eine Kopie von [`NumericValue_TO_AUI2_AND_STORE`](./NumericValue_TO_AUI2_AND_STORE.md), die zusätzlich den bestätigten Wert per M2M-Publish veröffentlicht: `AUI_SPLIT_2` wurde zu `AUI_SPLIT_3` erweitert, der dritte Zweig speist `AUI_PUBLISH_1`. Der M2M-Publish hängt damit am selben internen Confirm-Rückpfad wie die VT-Anzeige (`AUI_TO_AUDI`) und die INI-Persistenz (`INI_AUI`). Ein zusätzlicher externer Selfloop über einen `AUI2_TO_AUI`-Brückenbaustein ist nicht mehr nötig.

![NumericValue_TO_AUI2_AND_STORE_PUBLISH](NumericValue_TO_AUI2_AND_STORE_PUBLISH.svg)

## Schnittstelle

### Eingangsvariablen

- `QI` (BOOL, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst.
- `KEY` (STRING): Schlüsselname in der INI-Datei.
- `SECTION` (STRING): Abschnittsname in der INI-Datei.
- `u16ObjId` (UINT, Default `ID_NULL`): ObjectID des VT-Eingabefelds.
- `ID_WRITE` (WSTRING): Lokale Publish-Adresse (`ACTION=WRITE`) für den bestätigten Wert; M2M, wird remote abonniert.

### Ausgangsvariablen

- `QO` (BOOL): Quality Output - TRUE, wenn alle beteiligten Bausteine erfolgreich waren (UND-Verknüpfung über `Q_NumericValue_AUDI`, `NumericValue_IDA`, `INI_AUI`, `AUI_PUBLISH_1`).

### Adapter

- **Plug `VALUEO`** (`adapter::types::bidirectional::AUI2`): Wert zum Consumer und Echo zurück.

Der Baustein besitzt die Ereignisse `INIT` (Eingang) und `INITO` (Ausgang).

## Verwendete Funktionsbausteine (FBs)

- **NumericValue_IDA** (`isobus::UT::io::NumericValue::NumericValue_IDA`): liest das VT-Eingabefeld.
- **AD_TO_AUI** (`adapter::conversion::unidirectional::AD_TO_AUI`): wandelt den Wert in AUI.
- **AUI_LAST_2** (`adapter::events::unidirectional::AUI_LAST_2`): Last-Writer-Wins; beim Boot gewinnt der INI-Wert (`IN2`), danach jede Live-VT-Änderung (`IN1`).
- **AUI_TO_AUI2** (`adapter::conversion::bidirectional::AUI_TO_AUI2`): Brücke zum externen Consumer (z. B. `AOption_AUI_DEMUX_N.K`).
- **AUI_SPLIT_3** (`adapter::events::unidirectional::AUI_SPLIT_3`): dritter Zweig `OUT3` für `INI_AUI`, `OUT2` für `AUI_PUBLISH_1`, `OUT1` für die VT-Anzeige.
- **AUI_TO_AUDI** (`adapter::conversion::unidirectional::AUI_TO_AUDI`) und **Q_NumericValue_AUDI** (`isobus::UT::Q::Q_NumericValue_AUDI`): VT-Anzeige unter `u16ObjId`.
- **INI_AUI** (`eclipse4diac::storage::INI_AUI`): Parameter `SETM`=FALSE, `DEFAULT_VALUE`=`UINT#0`.
- **AUI_PUBLISH_1** (`adapter::net::AUI_PUBLISH_1`): M2M-Publish des bestätigten Werts auf `ID_WRITE`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_4`): verknüpft die vier `QO`.

## Funktionsweise

1. `NumericValue_IDA` liest das VT-Eingabefeld; `AD_TO_AUI` wandelt in AUI und liefert es an `AUI_LAST_2.IN1`.
2. `AUI_LAST_2` geht an `AUI_TO_AUI2`, dessen bidirektionaler Ausgang an `VALUEO`.
3. Das vom Consumer zurückgemeldete Ergebnis erscheint an `AUI_TO_AUI2.AUI_OUT` und wird von `AUI_SPLIT_3` verteilt: `OUT1` -> `AUI_TO_AUDI` -> `Q_NumericValue_AUDI.u32NewValue` (VT-Anzeige), `OUT2` -> `AUI_PUBLISH_1.IN` (M2M-Publish), `OUT3` -> `INI_AUI.AUI_IN` (Persistenz).
4. `INI_AUI.AUI_OUT` führt den geladenen Wert zu `AUI_LAST_2.IN2` zurück.
5. Initialisierungskette: `INIT` -> `NumericValue_IDA` -> `INI_AUI` -> `Q_NumericValue_AUDI` -> `AUI_PUBLISH_1` -> `INITO`. Mit `AUI_PUBLISH_1.INITO` wird zusätzlich `AND_QO.REQ` ausgelöst, dessen Ergebnis an `QO` geht.
6. `QI` wird an `Q_NumericValue_AUDI`, `NumericValue_IDA`, `INI_AUI` und `AUI_PUBLISH_1` verteilt.

## Technische Besonderheiten

- `SETM`=FALSE: `INI_AUI` quittiert nur beim Boot automatisch, nicht bei jedem späteren Schreibvorgang. Bei `SETM`=TRUE würde jeder Schreibvorgang ein neues Echo auslösen und einen Dauerkreislauf erzeugen (siehe auch [`NumericValue_TO_AUI2_AND_STORE`](./NumericValue_TO_AUI2_AND_STORE.md)).
- Der M2M-Publish liegt bewusst im bereits bewährten internen Rückpfad, damit er bei einem externen Umbau nicht verloren gehen kann.
- Der Baustein ist eine eigene Klasse; `NumericValue_TO_AUI2_AND_STORE` bleibt unverändert.

## Anwendungsszenarien

- Numerisches VT-Eingabefeld mit INI-Persistenz, bei dem der vom Consumer bestätigte Wert zusätzlich an andere Module veröffentlicht werden soll.

## Zusammenfassung

Wie `NumericValue_TO_AUI2_AND_STORE`, mit zusätzlichem M2M-Publish des bestätigten Werts am dritten Zweig von `AUI_SPLIT_3`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
