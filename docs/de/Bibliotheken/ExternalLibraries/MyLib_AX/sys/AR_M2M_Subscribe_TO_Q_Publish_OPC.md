# AR_M2M_Subscribe_TO_Q_Publish_OPC

![AR_M2M_Subscribe_TO_Q_Publish_OPC_network](./AR_M2M_Subscribe_TO_Q_Publish_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`AR_M2M_Subscribe_TO_Q_Publish_OPC` abonniert einen bereits fertig umgerechneten physikalischen Wert (REAL) per OPC-UA von einer anderen Station (M2M), zeigt ihn auf dem VT an (`Q_NumericValue_PHYSA`) und publiziert ihn lokal erneut, damit weitere lokale Konsumenten, Web-Mirror oder Dashboard ihn lesen können. Zusätzlich steht der Wert am Plug `VALUE_OUT` für lokale Adapter-Konsumenten bereit. Der Baustein ist die schlanke Nachfolgevariante von `AR_HZ_TO_Q_Publish_OPC` ohne eigene Skalierung, weil die Hz-zu-Einheit-Umrechnung inzwischen auf der Sensor-Station läuft (siehe [`logiBUS_PI_Speed_Publish_OPC`](logiBUS_PI_Speed_Publish_OPC.md)).

![AR_M2M_Subscribe_TO_Q_Publish_OPC](AR_M2M_Subscribe_TO_Q_Publish_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung; wird an `AR_SUBSCRIBE_M2M.INIT` weitergereicht

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung; kommt von `Publish.INITO`

### **Daten-Eingänge**

- **QI** (BOOL, Initialwert `TRUE`): Quality Input, schaltet den Dienst frei; wird an `AR_SUBSCRIBE_M2M.QI` und `Publish.QI` weitergereicht
- **ID_M2M_READ** (WSTRING): Remote-Subscribe-Adresse des fertigen physikalischen Werts (REAL), z. B. `FBs::Ventilsteuerung::Ventilsteuerung_STG1::RemoteSubStrings::AUFNAHME_GESCHWINDIGKEIT_KMH_M2M`
- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object-Pool-Eigenschaften der VT-Zahlenvariable (`u16ObjId`, Skalierung, Offset, Nachkommastellen), z. B. `FBs::const::DefaultPool_Numeric::NumberVariable_X_N`
- **ID_WRITE** (WSTRING): lokale Publish-Adresse (REAL, physikalischer Wert) für Web-Mirror, Dashboard und Self-Subscribe weiterer lokaler Konsumenten

### **Daten-Ausgänge**

- **QO** (BOOL): Quality Output, UND-Verknüpfung (`AND_QO`) der `QO`-Ausgänge von `AR_SUBSCRIBE_M2M` und `Publish`

### **Adapter**

- **VALUE_OUT** (Plug, `adapter::types::unidirectional::AR`): Nur-Lese-Spiegel des remote abonnierten Werts für weitere lokale Konsumenten; vermeidet einen doppelten OPC-UA-Self-Subscribe desselben Remote-Knotens auf dieser Station

## Funktionsweise

Das Netzwerk besteht aus vier Teilen:

- `AR_SUBSCRIBE_M2M` (`adapter::net::AR_SUBSCRIBE_1`, `QI` kommt vom Eingang `QI`): abonniert die Adresse `ID_M2M_READ` (`ID_M2M_READ` → `AR_SUBSCRIBE_M2M.ID`) und gibt den Wert als AR-Adapter aus.
- `AR_SPLIT_2_Value` (`adapter::events::unidirectional::AR_SPLIT_2`): verteilt den abonnierten Wert auf zwei Ziele (`AR_SUBSCRIBE_M2M.OUT` → `AR_SPLIT_2_Value.IN`).
- `Publish` (`MyLib::sys::AR_TO_Q_Publish_OPC`): zeigt den Wert am VT an und publiziert ihn lokal (`AR_SPLIT_2_Value.OUT1` → `Publish.AR_IN`, `stObj` → `Publish.stObj`, `ID_WRITE` → `Publish.ID_WRITE`).

- `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`): verknüpft `AR_SUBSCRIBE_M2M.QO` und `Publish.QO` zum Ausgang `QO`.

`AR_SPLIT_2_Value.OUT2` wird auf den Plug `VALUE_OUT` geführt.

Initialisierung und Qualität: `INIT` → `AR_SUBSCRIBE_M2M.INIT`, `AR_SUBSCRIBE_M2M.INITO` → `Publish.INIT`, `Publish.INITO` → `INITO` und `AND_QO.REQ`. `QI` wird an `AR_SUBSCRIBE_M2M.QI` und `Publish.QI` weitergereicht; `AR_SUBSCRIBE_M2M.QO` → `AND_QO.IN1`, `Publish.QO` → `AND_QO.IN2`, `AND_QO.OUT` → `QO`.

## Technische Besonderheiten

- Kein `SCALE`/`AR_MUL_2` mehr: Der abonnierte Wert liegt bereits in der physikalischen Einheit vor
- Der `AR_SPLIT_2` vor `Publish.AR_IN` wurde eingefügt, statt `AR_SUBSCRIBE_M2M.OUT` direkt zu verdrahten, damit ein Adapter nie ohne Split verzweigt wird
- `VALUE_OUT` liefert den abonnierten Wert für Verbraucher wie Befüllautomatik oder Füllband-Erkennung, ohne einen zweiten Subscribe auf denselben Remote-Knoten anzulegen

## Anwendungsszenarien

- Anzeige eines von einer anderen Station gemessenen Werts (z. B. Geschwindigkeit) am VT und gleichzeitige lokale Bereitstellung für Dashboard und Web-Client
- Weitergabe des Remote-Werts an lokale Regel- oder Automatik-Bausteine über `VALUE_OUT`

## Fazit

Der Baustein bündelt Remote-Subscribe, VT-Anzeige und lokalen Re-Publish eines fertigen physikalischen Werts in einer SubApp und stellt den Wert zusätzlich als AR-Plug bereit.
