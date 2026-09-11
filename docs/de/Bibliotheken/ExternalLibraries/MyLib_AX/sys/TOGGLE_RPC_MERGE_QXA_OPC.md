# TOGGLE_RPC_MERGE_QXA_OPC


![TOGGLE_RPC_MERGE_QXA_OPC_network](./TOGGLE_RPC_MERGE_QXA_OPC_network.svg)

![TOGGLE_RPC_MERGE_QXA_OPC](./TOGGLE_RPC_MERGE_QXA_OPC.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **TOGGLE_RPC_MERGE_QXA_OPC** realisiert einen „Klick-Toggle-Ausgang“ für den logiBUS. Ein einmaliger Tastendruck (z. B. auf einem SoftKey) schaltet einen physischen Ausgang dauerhaft ein oder aus – genau wie bei einem Blitzlicht. Der Trigger erfolgt über einen OPC-UA-Methodenaufruf (RPC), nicht über einen Wertwechsel. Der interne Zustand wird durch ein Toggle-Flipflop gehalten und mit einem bestehenden IO-Test-Kommando (Remote-Subscribe) logisch ODER-verknüpft, sodass der physische Ausgang sowohl durch die echte Funktion als auch durch den IO-Test angesteuert werden kann. Zusätzlich werden zwei getrennte Rückmeldekanäle über OPC-UA-Publish bereitgestellt: einer für die IO-Diagnose, einer für die eigentliche Funktionsrückmeldung (z. B. zur Hintergrundbeleuchtung eines SoftKeys).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | Physischer Ausgangswert (Initialwert: `logiBUS_DO::Invalid`) |
| `ID_TEST_READ` | `WSTRING` | Bestehende IO-Test-Subscribe-Adresse (z. B. `STG5_Q0x_READ`) |
| `ID_TEST_WRITE` | `WSTRING` | Bestehende IO-Test-Publish-Adresse (z. B. `STG5_Q0x_WRITE`) – eigener Knoten, nicht `ID_TEST_READ` (sonst Selbstkopplung/Latch) |
| `ID_TRIGGER_METHOD` | `WSTRING` | Lokale Methodenadresse (ACTION=CREATE_METHOD) für den argumentlosen Toggle-Trigger |
| `ID_STATE_WRITE` | `WSTRING` | Lokale Publish-Adresse (ACTION=WRITE) für den tatsächlichen Toggle-Zustand (remote abonniert, z. B. für GreenWhiteBackground) |

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

1. **Trigger-Empfang**: Ein OPC-UA-Client ruft über die durch `ID_TRIGGER_METHOD` definierte Methode den `SERVER_0`-Baustein (`TRIGGER_SERVER`) auf. Das Ereignis `IND` wird ausgelöst.
2. **Toggle-Flipflop**: Das Ereignis `IND` taktet das Flipflop `AX_T_FF`. Bei jedem Aufruf wechselt dessen Ausgang `Q` zwischen `TRUE` und `FALSE`. Gleichzeitig wird `IND` direkt auf `RSP` des Servers verdrahtet, um eine sofortige Bestätigung zu geben (keine Blockade des OPC-UA-Threads).
3. **Zusammenführung mit IO-Test**: Der Subskriptionsbaustein `SUBSCRIBE_TEST` empfängt kontinuierlich den Zustand des bestehenden IO-Test-Kanals (`ID_TEST_READ`) und gibt ihn als Booleschen Wert an `IN2` des ODER-Gatters (`OR_MERGE`) weiter. Der Flipflop-Zustand (`Q`) wird an `IN1` gelegt.
4. **ODER-Verknüpfung**: Der Ausgang des ODER-Gatters ist `TRUE`, wenn entweder der Toggle-Zustand oder das IO-Test-Kommando aktiv ist. Dadurch bleibt der physische Ausgang während eines IO-Tests unabhängig von der Toggle-Logik steuerbar.
5. **Signalverteilung**: Das ODER-Signal wird über den Splitter `SPLIT` an drei Ziele geführt:
   - `OUT1` → physischer Ausgang (`DigitalOutput`)
   - `OUT2` → IO-Test-Rückmeldung über `PUBLISH_TEST` (Adresse `ID_TEST_WRITE`)
   - `OUT3` → Funktions-Rückmeldung über `PUBLISH_STATE` (Adresse `ID_STATE_WRITE`)
6. **Publish**: Beide Publikationsbausteine senden den aktuellen Zustand als OPC-UA-Variable an die jeweiligen Adressen. Das Bedienmodul abonniert diese Werte für Anzeige oder Hintergrundbeleuchtung.

## Technische Besonderheiten

- **RPC-getriggert statt Pegel-getriggert**: Der Toggle-Impuls kommt über einen Methodenaufruf (`CALL_METHOD`), nicht über einen Wertwechsel. Dadurch ist die Funktion unabhängig von Signalflanken und benötigt kein Polling.
- **Kein aktives Client-Write für Rückmeldung**: Anders als das ursprüngliche Übungsmodell (`Toggle_RPC_FROM_Remote_QXA_OPC`) wird der Zustand nicht aktiv per `CLIENT_1_0` ans Bedienmodul geschrieben, sondern lokal publiziert und remote abonniert – konsistent mit allen anderen Funktionen des Projekts (z. B. `ILOCK_SWITCH_2_QXA_OPC`).
- **Getrennte Rückmeldeknoten**: Für IO-Test und Funktionszustand werden unterschiedliche OPC-UA-Knoten verwendet, um Selbstkopplung und Latch-Effekte zu vermeiden (siehe OPC_UA_VERNETZUNG.md, Punkte 10 und 11).
- **Sofortige RSP-Antwort**: Der `TRIGGER_SERVER` beantwortet den Methodenaufruf sofort (direkte Verbindung `IND → RSP`). Dies verhindert Verzögerungen des OPC-UA-Servers und blockiert nicht den globalen Service-Mutex.

## Zustandsübersicht

Die Subapp selbst besitzt keinen eigenen Zustandsautomaten. Der relevante Zustand wird im Toggle-Flipflop `AX_T_FF` gehalten:

- **Zustand Q = FALSE** (Aus): Der Ausgang ist inaktiv. Ein Trigger-Event setzt Q auf TRUE.
- **Zustand Q = TRUE** (Ein): Der Ausgang ist aktiv. Ein Trigger-Event setzt Q zurück auf FALSE.

Der physische Ausgang ist die ODER-Verknüpfung aus Q und dem IO-Test-Signal. Dadurch kann der Ausgang auch dann aktiv sein, wenn Q = FALSE, solange der IO-Test ein `TRUE` liefert.

## Anwendungsszenarien

- **Blitzlichtsteuerung**: Ein SoftKey wird kurz gedrückt, um das Licht ein- oder auszuschalten (Toggle). Der Trigger kommt per RPC, der Zustand wird zurückgemeldet.
- **Beleuchtungsumschaltung**: Umschalten zwischen zwei Betriebsmodi (z. B. Automatik/Manuell) über eine Taste.
- **Erweiterung vorhandener IO-Test-Funktionen**: Integration in bestehende logiBUS-Kanäle, ohne den IO-Test zu beeinträchtigen.

## Vergleich mit ähnlichen Bausteinen

- **MERGE_SWITCH_1_QXA_OPC**: Dieser Baustein schaltet wechselweise (tastend) – der Ausgang ist nur während des gedrückten Zustands aktiv. `TOGGLE_RPC_MERGE_QXA_OPC` erweitert dies um ein Flipflop, sodass der Zustand bis zum nächsten Tastendruck erhalten bleibt.
- **ILOCK_SWITCH_2_QXA_OPC**: Ähnlich wie `MERGE_SWITCH_1_QXA_OPC`, aber mit einer Verriegelungslogik. Der hier vorgestellte Baustein ist speziell für Klick-Toggle-Aktionen konzipiert und verwendet eine andere Trigger-Quelle (RPC-Methode statt Pegel-Subscription).
- **Toggle_RPC_FROM_Remote_QXA_OPC** (Übungsmodell): Das ursprüngliche Vorbild nutzt noch ein aktives Client-Write für die Rückmeldung; dieser Baustein wurde aus Konsistenzgründen auf das Publish/Subscribe-Muster umgestellt.

## Fazit

`TOGGLE_RPC_MERGE_QXA_OPC` bietet eine robuste und konsistente Lösung für Klick-Toggle-Funktionen auf Basis von OPC-UA-Methodenaufrufen. Durch die Kombination von Toggle-Flipflop und IO-Test-Einspeisung sowie die getrennten Rückmeldekanäle ist er flexibel in bestehende logiBUS-Umgebungen integrierbar und vermeidet typische Fehler wie Selbstkopplung oder Threadblockaden.
