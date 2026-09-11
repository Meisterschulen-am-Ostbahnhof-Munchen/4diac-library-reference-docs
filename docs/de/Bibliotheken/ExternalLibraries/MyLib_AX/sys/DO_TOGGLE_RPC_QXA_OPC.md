# DO_TOGGLE_RPC_QXA_OPC


![DO_TOGGLE_RPC_QXA_OPC_network](./DO_TOGGLE_RPC_QXA_OPC_network.svg)

![DO_TOGGLE_RPC_QXA_OPC](./DO_TOGGLE_RPC_QXA_OPC.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **DO_TOGGLE_RPC_QXA_OPC** realisiert einen digitalen Freigabe-Ausgang als Klick-Toggle. Er wird über einen RPC-Mechanismus (Open-S62541/CREATE_METHOD) von mehreren gleichberechtigten Aufrufern (z. B. SoftKey-Relais oder OPC-Dashboard) gesteuert. Der Ausgangszustand wird sowohl als physisches Signal (logiBUS-QXA) als auch als remote publizierter Wert (OPC-UA-Write) bereitgestellt. Die Besonderheit liegt darin, dass alle Aufrufer denselben Takt (CLK) auf ein Flipflop geben – ohne nachgelagerte ODER-Verknüpfung. Der Flipflop-Ausgang ist die alleinige „Source of Truth“ für den physischen Ausgang und den remote publizierten Zustand.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine (extern). Die auslösenden Ereignisse werden intern über den RPC-Server generiert.

### **Ereignis-Ausgänge**
Keine (extern).

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `Output_DO` | `logiBUS::io::DQ::logiBUS_DO_S` | Physischer Freigabe-Ausgang; steuert den digitalen Output (Klick-Toggle, Default AN). |
| `ID_DO_TOGGLE_METHOD` | `WSTRING` | Lokale Methodenadresse (ACTION=CREATE_METHOD) für den argumentlosen RPC-Trigger; wird von mehreren Aufrufern gemeinsam genutzt. |
| `ID_DO_STATE_WRITE` | `WSTRING` | Lokale Publish-Adresse (ACTION=WRITE) für den aktuellen Freigabe-Zustand (Flipflop-Q); wird remote abonniert. |

### **Daten-Ausgänge**
Keine.

### **Adapter**
Keine externen Adapter. Intern werden Adapter (AX_T_FF_INIT, AX_SPLIT_2, AX_PUBLISH_1) verwendet, um den Zustand zu verarbeiten und zu verteilen.

## Funktionsweise

Der Baustein besteht intern aus folgenden Funktionsblöcken:

- **DO_TOGGLE_SERVER** (`iec61499::net::SERVER_0`): Ein OPC-UA-Server, der eingehende RPC-Aufrufe auf der konfigurierten Methodenadresse (`ID_DO_TOGGLE_METHOD`) empfängt. Bei jedem Trigger wird das Ereignis `IND` ausgelöst.
- **AX_T_FF_INIT_DO** (`adapter::events::unidirectional::AX_T_FF_INIT`): Ein Flipflop mit initialem Ausgang `TRUE` (AN). Jedes `CLK`-Ereignis toggelt den Zustand.
- **SPLIT_DO_STATE** (`adapter::events::unidirectional::AX_SPLIT_2`): Verteilt den Zustand (Q) auf zwei parallele Ausgänge (OUT1 und OUT2).
- **DigitalOutput_DO** (`logiBUS::io::DQ::logiBUS_QXA`): Schaltet den physischen Ausgang entsprechend dem Flipflop-Zustand.
- **PUBLISH_STATE_DO** (`adapter::net::AX_PUBLISH_1`): Veröffentlicht den Zustand als OPC-UA-Write auf der Adresse `ID_DO_STATE_WRITE`.

**Ablauf:** Ein RPC-Trigger (z. B. vom OPC-Dashboard oder SoftKey) ruft die Methode auf. Der Server erzeugt ein `IND`-Ereignis, das direkt den Flipflop (CLK) taktet. Gleichzeitig wird `RSP` sofort zurückgesendet, um den Server-Thread nicht zu blockieren (siehe Technische Besonderheiten). Der Flipflop-Ausgang `Q` wechselt seinen Zustand (toggle). Über den Splitter wird dieser Zustand gleichzeitig an den physischen Ausgang und an den Publisher übergeben. Der Publisher aktualisiert den remote überwachten Wert.

## Technische Besonderheiten

- **Rückverdrahtung RSP → IND:** Der Server muss `RSP` unmittelbar auf `IND` zurückführen. Ohne diese direkte Verbindung blockiert FORTE den OPC-UA-Server-Thread bis zu 4 Sekunden (Methode-Timeout) und hält dabei den globalen open62541-Mutex, was auch andere OPC-UA-Kommunikation (z. B. Publish) behindern kann. Die direkte RSP-Verdrahtung ist zwingend erforderlich.
- **Keine ODER-Verknüpfung nach dem Flipflop:** Anders als ähnliche Bausteine (z. B. `TOGGLE_RPC_MERGE_QXA_OPC`) wird hier kein OR nach dem Flipflop verwendet. Da alle Aufrufer denselben CLK-Eingang des Flipflops speisen, ist keine Zusammenführung auf Datenebene notwendig. Dadurch bleibt die Zustandslogik eindeutig und einfach.
- **Eine einzige „Source of Truth“:** Der Ausgang von `AX_T_FF_INIT_DO.Q` bestimmt sowohl den physischen als auch den remote publizierten Zustand. Es gibt keine Abweichungen oder Doppelquellen.

## Zustandsübersicht

| Zustand | Beschreibung |
|---------|--------------|
| `TRUE` (AN) | Freigabe aktiv (Default). Physischer Ausgang und OPC-Wert sind `TRUE`. |
| `FALSE` (AUS) | Freigabe inaktiv. Physischer Ausgang und OPC-Wert sind `FALSE`. |

Jeder RPC-Trigger wechselt zwischen diesen Zuständen. Der Initialzustand ist `TRUE` (AN).

## Anwendungsszenarien

- **Not-Bedienung:** Ein manueller Schalter (SoftKey) und ein OPC-Dashboard können abwechselnd oder gleichzeitig denselben digitalen Ausgang (z. B. Freigabe für eine Maschine) per Toggle aktivieren/deaktivieren.
- **Straßenmodus / Verkehrssteuerung:** Umschalten zwischen „Freigabe“ und „Sperrung“ über mehrere Bedienstellen.
- **Generische digitale Toggle-Ausgänge** mit Mehrfach-Bedienung, bei denen eine konsistente Zustandsverwaltung ohne zusätzliche Logik gefordert ist.

## Vergleich mit ähnlichen Bausteinen

Der Baustein `DO_TOGGLE_RPC_QXA_OPC` unterscheidet sich von Bausteinen wie `TOGGLE_RPC_MERGE_QXA_OPC` (z. B. für Blitzersteuerung) dadurch, dass er **kein ODER** nach dem Flipflop verwendet. In einem Szenario, in dem mehrere Aufrufer potenziell unterschiedliche Zustände anlegen könnten, wäre ein OR notwendig. Hier jedoch wird nichts gemerged – alle Aufrufer takten denselben Flipflop. Dies vereinfacht die Logik, reduziert Latenz und vermeidet Konsistenzprobleme, da es keine parallelen Pfade gibt.

## Fazit

Der Baustein **DO_TOGGLE_RPC_QXA_OPC** ist eine saubere, effiziente Lösung für digitale Freigabe-Ausgänge, die über RPC von mehreren Stellen getoggelt werden. Durch die Verwendung eines einzigen Flipflops als Quelle wird sichergestellt, dass der physische und der kommunizierte Zustand immer übereinstimmen. Die direkte RSP-Rückkopplung verhindert Blockaden im OPC-UA-Server und gewährleistet eine schnelle Reaktionszeit. Die einfache Struktur erleichtert Wartung und Wiederverwendung in verschiedenen Anwendungen.