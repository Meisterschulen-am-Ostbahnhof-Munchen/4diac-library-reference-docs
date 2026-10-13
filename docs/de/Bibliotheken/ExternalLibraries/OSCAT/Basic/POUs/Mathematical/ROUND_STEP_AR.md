# ROUND_STEP_AR

![ROUND_STEP_AR_network](./ROUND_STEP_AR_network.svg)

* * * * * * * * * *

## Einleitung

`ROUND_STEP_AR` rundet `AR_IN` auf ein **beliebiges Raster** `RASTER` (z. B. 0.25), nicht nur auf Nachkommastellen wie [`ROUND_AR`](./ROUND_AR.md). Rechenweg: `ROUND(AR_IN / RASTER) * RASTER`. Der Baustein ist vorerst eine SubApp, die das bestehende `ROUND_AR` wiederverwendet; später ggf. als eigener Basic-Baustein.

![ROUND_STEP_AR](ROUND_STEP_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

- **RASTER** (`REAL`): Rundungsraster, z. B. `0.25`. Platzhalter-Default `1.0` (= Rundung auf ganze Zahl), per Parameter überschreibbar.

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **AR_IN** (Socket, `adapter::types::unidirectional::AR`): Eingangssignal (`REAL`)
- **AR_OUT** (Plug, `adapter::types::unidirectional::AR`): auf `RASTER` gerundeter Wert (`REAL`)

## Verwendete Funktionsbausteine (FBs)

- **`LIT_Step`** (`adapter::types::unidirectional::AR::initval::initval_AR`): erzeugt aus `RASTER` einen konstanten `AR`-Wert
- **`AR_SPLIT_2_Step`** (`adapter::events::unidirectional::AR_SPLIT_2`): fächert `RASTER` auf Division und Multiplikation auf (Adapter-Fanout ist strikt 1:1)
- **`AR_DIV_Step`** (`adapter::iec61131::arithmetic::AR_DIV_2`): `AR_IN / RASTER`
- **`Round0`** (`OSCAT_adapter::Mathematical::ROUND_AR`, `N = 0`): rundet den Quotienten auf eine ganze Zahl
- **`AR_MUL_Step`** (`adapter::iec61131::arithmetic::AR_MUL_2`): multipliziert zurück mit `RASTER`

## Funktionsweise

- `RASTER` → `LIT_Step.INIT_VAL`; `LIT_Step.OUT` → `AR_SPLIT_2_Step.IN`
- `AR_IN` → `AR_DIV_Step.IN1`; `AR_SPLIT_2_Step.OUT1` → `AR_DIV_Step.IN2`
- `AR_DIV_Step.OUT` → `Round0.AR_IN`
- `Round0.AR_OUT` → `AR_MUL_Step.IN1`; `AR_SPLIT_2_Step.OUT2` → `AR_MUL_Step.IN2`
- `AR_MUL_Step.OUT` → `AR_OUT`

## Technische Besonderheiten

- Rundung auf ein Raster statt auf Nachkommastellen
- `RASTER` wird per Parameter bei der Instanziierung gesetzt
- Beispiel: `RASTER = 0.25` rundet auf Viertel

## Anwendungsszenarien

- Rundung von Sollwerten oder Anzeigewerten auf Schrittweiten (z. B. 0.25, 0.5, 5)

## ⚖️ Vergleich mit ähnlichen Bausteinen

- [`ROUND_AR`](./ROUND_AR.md) rundet nur auf `N` Nachkommastellen und wird hier intern verwendet.

## Zusammenfassung

`ROUND_STEP_AR` rundet ein `AR`-Signal auf ein frei wählbares Raster.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
