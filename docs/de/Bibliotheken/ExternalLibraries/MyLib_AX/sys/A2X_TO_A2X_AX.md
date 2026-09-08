# A2X_TO_A2X_AX

![A2X_TO_A2X_AX_network](./A2X_TO_A2X_AX_network.svg)

* * * * * * * * * *

## Einleitung

`A2X_TO_A2X_AX` ist das gebündelte Pendant zu `AX_2_TO_3`: Statt zwei getrennter `AX`-Sockets/Plugs (`UP_IN`/`DOWN_IN`, `UP_OUT`/`DOWN_OUT`) nimmt dieser Baustein UP/DOWN gebündelt als ein einziges `A2X`-Signal entgegen, reicht es unverändert durch und liefert zusätzlich ein einzelnes `AX`-Signal mit dem ODER aus beiden Richtungen (z. B. für ein gemeinsames Treiber-Enable).

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: A2X_TO_A2X_AX

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **UNBUNDLE**: `adapter::conversion::unidirectional::A2X_2X_TO_2AX` — trennt das eingehende `A2X_IN` in zwei einzelne `AX`-Signale (UP, DOWN).
    - **AX_SPLIT_UP** / **AX_SPLIT_DOWN**: je `adapter::events::unidirectional::AX_SPLIT_2` — verzweigen je eine Richtung in einen Rückweg (zurück ins gebündelte Ausgangssignal) und einen Zweig zur ODER-Verknüpfung.
    - **BUNDLE**: `adapter::conversion::unidirectional::A2X_2AX_TO_2X` — fügt UP/DOWN wieder zu einem einzigen `A2X_OUT` zusammen.
    - **AX_OR_2**: `adapter::booleanOperators::AX_OR_2` — liefert auf `OR_OUT`, ob der Aktor gerade in irgendeine Richtung läuft.
- **Funktionsweise**: `A2X_IN` wird entbündelt, jede Richtung wird gesplittet (einmal zurück in die Bündelung, einmal in die ODER-Verknüpfung), UP/DOWN werden wieder gebündelt (`A2X_OUT`), und das ODER beider Richtungen liegt zusätzlich auf `OR_OUT`.

## Programmablauf und Verbindungen

1. `A2X_IN` → `UNBUNDLE.A2X_IN` → `UNBUNDLE.UP` → `AX_SPLIT_UP.IN`; `UNBUNDLE.DOWN` → `AX_SPLIT_DOWN.IN`.
2. `AX_SPLIT_UP.OUT1` → `BUNDLE.UP`; `AX_SPLIT_DOWN.OUT1` → `BUNDLE.DOWN`; `BUNDLE.A2X_OUT` → `A2X_OUT`.
3. `AX_SPLIT_UP.OUT2` → `AX_OR_2.IN1`; `AX_SPLIT_DOWN.OUT2` → `AX_OR_2.IN2`; `AX_OR_2.OUT` → `OR_OUT`.

## Technische Besonderheiten

- **Unveränderte Durchreichung trotz Bündelung**: Nach außen bleibt `A2X` gebündelt, intern wird UP/DOWN wie bei `AX_2_TO_3` unverändert durchgereicht.
- **Je ein Split pro Richtung**: Notwendig, weil jedes der beiden entbündelten Signale zu zwei Zielen muss — zurück in die Bündelung und in die ODER-Verknüpfung.

## Anwendungsszenarien

- Schaltungen, in denen UP/DOWN ohnehin als gebündeltes `A2X`-Signal geführt werden (z. B. Richtung an einem Doppelwirkungsventil) und nicht extra in zwei einzelne `AX`-Signale aufgesplittet werden sollen.

## Vergleich mit ähnlichen Bausteinen

Werden UP/DOWN ohnehin einzeln als `AX` geführt, ist stattdessen `AX_2_TO_3` zu verwenden — funktional identisch (Durchreichen plus ODER als drittes Signal), aber mit ungebündelter Schnittstelle.

## Zusammenfassung

`A2X_TO_A2X_AX` reicht ein gebündeltes `A2X`-Signal unverändert durch und ergänzt es um ein zusätzliches `AX`-ODER-Signal — das gebündelte Gegenstück zu `AX_2_TO_3`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
