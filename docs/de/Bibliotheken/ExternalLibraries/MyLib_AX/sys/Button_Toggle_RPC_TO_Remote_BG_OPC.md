# Button_Toggle_RPC_TO_Remote_BG_OPC


![Button_Toggle_RPC_TO_Remote_BG_OPC_network](./Button_Toggle_RPC_TO_Remote_BG_OPC_network.svg)

![Button_Toggle_RPC_TO_Remote_BG_OPC](./Button_Toggle_RPC_TO_Remote_BG_OPC.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `Button_Toggle_RPC_TO_Remote_BG_OPC` ist eine Subapplikation (SubApp) zur Realisierung eines Taster-basierten Remote-Prozeduraufrufs (RPC) über OPC-UA. Er dient als Analogie zum vorhandenen Softkey-Baustein `Softkey_Toggle_RPC_TO_Remote_BG_OPC`, ist jedoch für den Einsatz mit IO-Diagnose-Tastern (z. B. CButton) konzipiert. Der Baustein reagiert auf das Ereignis „Loslassen“ (BT_RELEASED_UNLATCHED) eines Tasters und löst daraufhin einen argument- und rückgabewertlosen OPC-UA-Methodenaufruf auf einem entfernten Zielmodul aus. Zusätzlich überwacht er den Zustand eines Flipflops auf dem Zielmodul und visualisiert diesen über eine Hintergrundfarb-Darstellung (GreenWhiteBackground). Das Protokoll ist identisch zum Softkey-Pendant, verwendet jedoch eine Taster-Eingangsschnittstelle (Button_IE) statt einer Softkey-Schnittstelle.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Baustein besitzt keine externen Ereignis-Eingänge. Die Ereignisauslösung erfolgt intern über den verwendeten `Button_IE`-Funktionsbaustein, der auf das Tasterereignis `BT_RELEASED_UNLATCHED` reagiert.

### **Ereignis-Ausgänge**

Es sind keine Ereignis-Ausgänge vorhanden. Der Baustein ist als reiner Trigger konzipiert; die Ergebnisrückmeldung erfolgt über den überwachten Zustand.

### **Daten-Eingänge**

| Name | Typ | Initialwert | Kommentar |
|------|-----|-------------|-----------|
| `u16ObjId` | UINT | `ID_NULL` | Objekt-ID des Tasters bzw. der Hintergrundanzeige. Wird sowohl an den Taster-Baustein als auch an die Anzeige-SubApp weitergegeben. |
| `ID_TRIGGER_CALL` | WSTRING | – | Remote-Methodenadresse (ACTION=CALL_METHOD) für den argumentlosen Trigger-Methodenaufruf auf dem Zielmodul. |
| `ID_STATE_READ` | WSTRING | – | Lokal überwachte Adresse (BOOL, ACTION=READ) für den Flipflop-Zustand, der vom Zielmodul remote beschrieben wird. |

### **Daten-Ausgänge**

Es sind keine Daten-Ausgänge vorhanden.

### **Adapter**

Der Baustein verwendet intern einen Adapter `AX_SUBSCRIBE_1` für die Abonnement-Übertragung des Zustandswerts. Von außen ist kein Adapter sichtbar, da die Verbindung intern verdrahtet ist.

## Funktionsweise

Der Baustein kombiniert mehrere Funktionsbausteine und SubApplikationen:

1. **Taster-Erkennung**: Der FB `Button_IE` (vom Typ `isobus::UT::io::Button::Button_IE`) wird mit dem Aktivierungscode `BT_RELEASED_UNLATCHED` konfiguriert. Dieser reagiert auf das Loslassen eines Tasters (unverriegelt) und erzeugt ein Ereignis am Ausgang `IND`.

2. **Remote-Call-Triggerung**: Das Ereignis `IND` wird mit dem Eingang `REQ` des FB `TRIGGER_CLIENT` (vom Typ `iec61499::net::CLIENT_0`) verbunden. Der Client führt einen OPC-UA-Methodenaufruf gemäß der übergebenen Adresse `ID_TRIGGER_CALL` aus. Da keine Parameter und Rückgabewerte vorhanden sind, dient dies als reiner RPC-Trigger.

3. **Zustandsüberwachung**: Der FB `STATE_SUBSCRIBE` (Typ `adapter::net::AX_SUBSCRIBE_1`) abonniert die über `ID_STATE_READ` spezifizierte Bool-Adresse auf dem Zielmodul. Der empfangene Zustand wird über den Adapterausgang `OUT` an die SubApp `GreenWhiteBackground_AX` weitergegeben.

4. **Hintergrundvisualisierung**: Die SubApp `GreenWhiteBackground_AX` (Typ `MyLib::sys::GreenWhiteBackground1_AX`) empfängt den Zustand über den Adapter `DI1` und stellt den Flipflop-Zustand farblich dar (z. B. grün für aktiv, weiß für inaktiv).

Die Verbindung zwischen `u16ObjId` und den internen Bausteinen stellt sicher, dass die korrekte Objekt-ID sowohl für den Taster als auch für die Anzeige verwendet wird.

## Technische Besonderheiten

- **Identisches Protokoll**: Der Baustein verwendet exakt dasselbe Kommunikationsprotokoll wie der Softkey-Baustein `Softkey_Toggle_RPC_TO_Remote_BG_OPC`, nur die Eingangsschnittstelle unterscheidet sich (Button statt Softkey).
- **Keine Örtliche Logik**: Im Gegensatz zu einem lokalen Toggle-Flipflop (z. B. `AX_T_FF`) wird der Flipflop-Zustand ausschließlich auf dem Zielmodul gehalten und überwacht.
- **RPC ohne Argmente**: Der Methodenaufruf erfolgt ohne Argumente und Rückgabewerte – reiner Trigger.
- **SubApp-Kapselung**: Die gesamte Funktionalität ist in einer SubApp gekapselt, was Wiederverwendung und Übersichtlichkeit erhöht.
- **Attribut „Visible“**: Die internen Datenverbindungen sind mit `Visible=false` markiert, um die Netzansicht in der IDE zu verschlanken.

## Zustandsübersicht

Der Baustein selbst besitzt keinen expliziten Zustandsautomaten. Der überwachte Flipflop-Zustand (Bool) kann zwei Werte annehmen:

- **TRUE**: Taster gedrückt/aktiviert → Hintergrund grün
- **FALSE**: Taster losgelassen/deaktiviert → Hintergrund weiß

Die Zustandsänderung wird durch den OPC-UA-Aufruf am Zielmodul ausgelöst und über das Abonnement zurückgelesen.

## Anwendungsszenarien

- **Remote-Toggle-Schalter**: Ein Taster an einer IO-Diagnose-Maske soll einen Flipflop auf einem entfernten Steuerungsmodul umschalten und den aktuellen Zustand vor Ort anzeigen.
- **Freigabeschalter**: In einer Fertigungsanlage können Taster zum Ein-/Ausschalten von Funktionen (z. B. Aufnahme links/rechts) verwendet werden, wobei der Zustand auf dem Zielmodul gehalten wird.
- **Bedienpanel**: Einsatz in Bedienoberflächen, bei denen physische Taster statt Softkeys verwendet werden, aber das gleiche Kommunikationsprotokoll wie bei Softkeys genutzt werden soll.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Eingangsschnittstelle | Verwendung |
|----------|------------------------|------------|
| `Button_Toggle_RPC_TO_Remote_BG_OPC` | Button_IE | Taster an IO-Diag-DataMask |
| `Softkey_Toggle_RPC_TO_Remote_BG_OPC` | Softkey_IE | Softkey an SoftkeyMask |

Beide Bausteine nutzen das gleiche RPC-Trigger- und Zustandsüberwachungsprinzip, unterscheiden sich jedoch in der Art der Bedienung (physischer Taster vs. Softkey).

Ein alternativer Ansatz wäre ein lokales Flipflop (z. B. `AX_T_FF`), bei dem der Zustand nicht remote sondern lokal gehalten wird. Dies würde jedoch zusätzliche Synchronisationslogik erfordern.

## Fazit

Der Baustein `Button_Toggle_RPC_TO_Remote_BG_OPC` bietet eine kompakte und wiederverwendbare Lösung für die Anbindung physischer Taster an eine OPC-UA-basierte Remote-Steuerung. Durch die Kapselung in eine SubApp und die Verwendung standardisierter Schnittstellen (`CLIENT_0`, `AX_SUBSCRIBE_1`) ist er flexibel und in verschiedene Automatisierungssysteme integrierbar. Die klare Trennung zwischen Trigger und Zustandsvisualisierung erleichtert Wartung und Anpassung an unterschiedliche Zielmodule.
