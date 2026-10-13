# AR_TO_Q_Publish_OPC

![AR_TO_Q_Publish_OPC_network](./AR_TO_Q_Publish_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`AR_TO_Q_Publish_OPC` führt einen physikalischen `AR`-Wert aus **einer gemeinsamen Quelle** sowohl auf eine VT-Zahlenanzeige (`Q_NumericValue_PHYSA`) als auch auf einen OPC-UA-Publish (`AR_PUBLISH_1`). Sie ist für Werte gedacht, die am VT angezeigt und zusätzlich lokal (Self-Subscribe) oder stationsübergreifend weiterverwendet werden sollen. Herausgelöst aus `TECU_Speeds`/`TECU_RPTO`, wo die Kombination `AR_SPLIT_2` + `Q_NumericValue_PHYSA` + `AR_PUBLISH_1` häufig vorkam.

![AR_TO_Q_Publish_OPC](AR_TO_Q_Publish_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): Objekt-Pool-Eigenschaften der VT-Zahlenvariable (`u16ObjId`, Skalierung, Offset, Nachkommastellen), z. B. `FBs::const::DefaultPool_Numeric::NumberVariable_X_N`
- **ID_WRITE** (`WSTRING`): Publish-Adresse (REAL, `ACTION=PUBLISH`), lokal oder stationsübergreifend

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **AR_IN** (Socket, `adapter::types::unidirectional::AR`): physikalischer Wert (bereits skaliert)

## Verwendete Funktionsbausteine (FBs)

- **`RateLimit_Anzeige`** (`adapter::events::unidirectional::AR_D_FF_TMIN`, `Tmin = T#500ms`): begrenzt die Update-Rate
- **`AR_SPLIT_2`** (`adapter::events::unidirectional::AR_SPLIT_2`): verteilt den Wert auf zwei Ziele
- **`Q_NumericValue`** (`isobus::UT::Q::Q_NumericValue_PHYSA`): schreibt den physikalischen Wert in die VT-Zahlenvariable
- **`AR_PUBLISH`** (`adapter::net::AR_PUBLISH_1`, `QI = TRUE`): publiziert den Wert per OPC-UA

## Funktionsweise

- `AR_IN` → `RateLimit_Anzeige.I`; `RateLimit_Anzeige.Q` → `AR_SPLIT_2.IN`
- `AR_SPLIT_2.OUT1` → `Q_NumericValue.rPhys` (VT-Anzeige); `stObj` → `Q_NumericValue.stObj`
- `AR_SPLIT_2.OUT2` → `AR_PUBLISH.IN`; `ID_WRITE` → `AR_PUBLISH.ID`

## Technische Besonderheiten

- **Ratenbegrenzung vor dem Split (Absicht):** Seit v1.2 sitzt `RateLimit_Anzeige` vor `AR_SPLIT_2` und drosselt damit **beide** Zweige (VT-Anzeige und Publish/Self-Subscribe-Konsumenten wie `CTRL_PID_AR.AR_ACT`) auf höchstens ein Update je 500 ms. In v1.1 war die Drossel nur im VT-Anzeige-Zweig. Anlass war ein träges Verhalten bei vielen gleichzeitig schnell wechselnden Anzeigen (vermutete VT-Bus-Last).
- Ein Adapter darf nicht auf zwei Ziele fan-outen, daher der explizite `AR_SPLIT_2`.

## Anwendungsszenarien

- Drehzahl-, Geschwindigkeits- oder andere Messwerte, die am VT angezeigt und zugleich lokal oder an andere Stationen veröffentlicht werden

## Zusammenfassung

`AR_TO_Q_Publish_OPC` bündelt Ratenbegrenzung, Aufteilung, VT-Anzeige und OPC-UA-Publish eines `AR`-Wertes in einer SubApp.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
