# NumericValue_TO_CLIENT_1_0_OPC

![NumericValue_TO_CLIENT_1_0_OPC_network](./NumericValue_TO_CLIENT_1_0_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`NumericValue_TO_CLIENT_1_0_OPC` verbindet ein VT-Eingabefeld (`CInputNumber`) mit einer entfernten **OPC-UA-Methode**: **Nur bei einer echten Eingabe** (`NumericValue_PHYSA` feuert beim Commit) wird die Methode (`CALL_METHOD`, `AR_CLIENT_1_0`) mit dem eingegebenen `REAL`-Wert als Argument aufgerufen. Der Rückweg – Anzeige des tatsächlich wirksamen, bestätigten Werts auf demselben VT-Feld – läuft separat und unkritisch über einen normalen Subscribe (`Q_NumericValue_PHYSA`). Es gibt dadurch keinen Zirkelbezug und kein Merge/Last-Writer-Wins: Auf der Empfängerseite gibt es nur einen Aufrufer pro Methode, serialisiert durch den Methodenaufruf selbst.

![NumericValue_TO_CLIENT_1_0_OPC](NumericValue_TO_CLIENT_1_0_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden*

### **Ereignis-Ausgänge**

*Keine direkten Ereignis-Ausgänge vorhanden*

### **Daten-Eingänge**

- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object-ID und Scale/Offset/Decimals des VT-Eingabefelds; wird für Ein- und Ausgabe verwendet (der bestätigte Wert wird hier zurückgeschrieben). Vorgabe: `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 1)`
- **ID_CALL** (`WSTRING`): Remote-Methodenadresse (`ACTION=CALL_METHOD`); wird nur bei einer echten VT-Eingabe mit dem eingegebenen Wert als Argument aufgerufen
- **ID_M2M_READ** (`WSTRING`): Remote-Subscribe-Adresse (`REAL`, `ACTION=SUBSCRIBE`); liefert den tatsächlich wirksamen Wert zur Anzeige auf demselben VT-Feld zurück (reines Lesen)

### **Daten-Ausgänge**

*Keine Daten-Ausgänge vorhanden*

### **Adapter**

Dieser Funktionsblock verwendet keine Adapter-Sockets/-Plugs an der Schnittstelle; intern werden `AR`-Adapter verwendet.

## Verwendete Funktionsbausteine (FBs)

- **Input**: `isobus::UT::io::NumericValue::NumericValue_PHYSA` (`QI=TRUE`) – liefert den eingegebenen physikalischen Wert (`rPhys`) beim Commit
- **Call**: `adapter::net::AR_CLIENT_1_0_UNGATED` (`QI=TRUE`) – ruft die Remote-Methode mit dem Wert als Argument auf
- **Subscribe**: [`adapter::net::AR_SUBSCRIBE_1`](../../adapter/net/AR_SUBSCRIBE_1.md) (`QI=TRUE`) – abonniert den bestätigten Wert
- **Display**: `isobus::UT::Q::Q_NumericValue_PHYSA` – schreibt den Wert auf das VT-Feld

## Programmablauf und Verbindungen

1. **Hinweg**: `Input.rPhys` → `Call.IN` (Eingabe → Methodenaufruf).
2. **Rückweg**: `Subscribe.OUT` → `Display.rPhys` (bestätigter Wert → VT-Anzeige).
3. `Subscribe.INITO` → `Call.INIT`: Der Client wird erst nach der Initialisierung des Subscribe initialisiert.
4. `stObj` → `Input.stObj` und `Display.stObj`; `ID_CALL` → `Call.ID`; `ID_M2M_READ` → `Subscribe.ID`.

## Technische Besonderheiten

- Hin- und Rückweg sind getrennt, deshalb ist kein Merge nötig.
- `AR_CLIENT_1_0_UNGATED` ist die ungegatete Variante des Clients.

## Anwendungsszenarien

- Sollwert-Eingabe am VT, die auf einer anderen Station per OPC-UA-Methode wirksam wird, mit Rückmeldung des tatsächlich gültigen Werts

## Fazit

`NumericValue_TO_CLIENT_1_0_OPC` schreibt VT-Eingaben per OPC-UA-Methodenaufruf zur Zielstation und zeigt den bestätigten Wert unabhängig davon über einen Subscribe an.
