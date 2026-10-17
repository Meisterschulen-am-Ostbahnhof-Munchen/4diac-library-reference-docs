# AR_M2M_Subscribe_TO_Q_OPC

![AR_M2M_Subscribe_TO_Q_OPC_network](./AR_M2M_Subscribe_TO_Q_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`AR_M2M_Subscribe_TO_Q_OPC` abonniert per Remote-Subscribe (M2M) einen fertigen physikalischen Wert (`REAL`) und zeigt ihn auf der VT an (`Q_NumericValue_PHYSA`, auf höchstens ein Update pro 500 ms gedrosselt). Zusätzlich steht der Wert als Nur-Lese-Spiegel am Plug `VALUE_OUT` für weitere lokale Konsumenten bereit. Der Baustein entspricht [`AR_M2M_Subscribe_TO_Q_Publish_OPC`](./AR_M2M_Subscribe_TO_Q_Publish_OPC.md), jedoch ohne lokalen Re-Publish (kein `ID_WRITE`) – die schlanke Lib-Variante.

![AR_M2M_Subscribe_TO_Q_OPC](AR_M2M_Subscribe_TO_Q_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung; startet die Kette `AR_SUBSCRIBE_M2M` → `Q_NumericValue`

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung; kommt von `Q_NumericValue.INITO`

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `QI` | BOOL | TRUE | Quality Input: Freigabe des Dienstes; wird an `AR_SUBSCRIBE_M2M` und `Q_NumericValue` weitergereicht. |
| `ID_M2M_READ` | WSTRING |  | Remote-Subscribe-Adresse des fertigen physikalischen Werts (`REAL`). |
| `stObj` | `logiBUS::utils::conversion::phys::NumericObjectPool_S` | `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 0)` | Object-Pool-Eigenschaften der VT-Zahlenvariable (`u16ObjId`, Scale, Offset, Decimals). |

### **Daten-Ausgänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `QO` | BOOL |  | Quality Output: UND aus `AR_SUBSCRIBE_M2M.QO` und `Q_NumericValue.QO`. |

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `VALUE_OUT` | `adapter::types::unidirectional::AR` | Plug | Nur-Lese-Spiegel des remote abonnierten Werts für weitere lokale Konsumenten (ungedrosselt). |

## Verwendete Funktionsbausteine (FBs)

- **AR_SUBSCRIBE_M2M** (`adapter::net::AR_SUBSCRIBE_1`): Remote-Subscribe des Werts
- **AR_SPLIT_2_Value** (`adapter::events::unidirectional::AR_SPLIT_2`): teilt den Wert auf Anzeige-Zweig und `VALUE_OUT`
- **RateLimit_Anzeige** (`adapter::events::unidirectional::AR_D_FF_TMIN`, `Tmin = T#500ms`): drosselt nur die VT-Anzeige auf höchstens ein Update pro `Tmin`
- **Q_NumericValue** (`isobus::UT::Q::Q_NumericValue_PHYSA`): schreibt den Wert in die VT-Zahlenvariable
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): verknüpft die beiden `QO`-Ausgänge

## Programmablauf und Verbindungen

1. Initialisierung: `INIT` -> `AR_SUBSCRIBE_M2M.INIT`; `AR_SUBSCRIBE_M2M.INITO` -> `Q_NumericValue.INIT`; `Q_NumericValue.INITO` -> `INITO` und -> `AND_QO.REQ`.
2. Daten: `ID_M2M_READ` -> `AR_SUBSCRIBE_M2M.ID`; `stObj` -> `Q_NumericValue.stObj`.
3. Qualität: `QI` -> `AR_SUBSCRIBE_M2M.QI` und `Q_NumericValue.QI`; `AR_SUBSCRIBE_M2M.QO` -> `AND_QO.IN1`; `Q_NumericValue.QO` -> `AND_QO.IN2`; `AND_QO.OUT` -> `QO`.
4. Wertpfad: `AR_SUBSCRIBE_M2M.OUT` -> `AR_SPLIT_2_Value.IN`.
5. Anzeige-Zweig: `AR_SPLIT_2_Value.OUT1` -> `RateLimit_Anzeige.I`; `RateLimit_Anzeige.Q` -> `Q_NumericValue.rPhys`.
6. Spiegel-Zweig: `AR_SPLIT_2_Value.OUT2` -> `VALUE_OUT`.

## Technische Besonderheiten

- Die Drosselung (`T#500ms`) wirkt nur auf dem Anzeige-Zweig; `VALUE_OUT` bleibt ungedrosselt wie im Original.
- Gegenüber [`AR_M2M_Subscribe_TO_Q_Publish_OPC`](./AR_M2M_Subscribe_TO_Q_Publish_OPC.md) entfallen der lokale Publish und `ID_WRITE`.

## Anwendungsszenarien

- Anzeige eines von einer anderen Station gelieferten, bereits physikalisch skalierten Werts auf der VT, wenn kein lokaler Re-Publish benötigt wird.
- Weitergabe des abonnierten Werts über `VALUE_OUT` an weitere lokale Konsumenten.

## Zusammenfassung

`AR_M2M_Subscribe_TO_Q_OPC` abonniert einen fertigen physikalischen Wert remote, zeigt ihn gedrosselt auf der VT an und stellt ihn zusätzlich über `VALUE_OUT` bereit – ohne lokalen Re-Publish.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
