# Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter


![Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter_network](./Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter_network.svg)

![Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter](./Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter` ist eine Subapplikation (SubApp) zur Anbindung einer lokalen HMI (Softkeys mit Green‑White‑Background) an ein entferntes Gerät (Gerät B) über OPC‑UA. Die SubApp entkoppelt die HMI‑Logik von der OPC‑UA‑Trigger‑Funktionalität und ermöglicht so eine flexible Integration in Automatisierungsumgebungen. Sie wird auf Gerät A (z. B. Station 11, IP 192.168.1.11) eingesetzt und kommuniziert mit Gerät B über Remote‑Methodenaufrufe.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

| Name            | Typ      | Beschreibung                                                                 |
|-----------------|----------|------------------------------------------------------------------------------|
| `u16ObjId_SET`  | UINT     | Object ID für den SoftKey "Set" (Standard: `ID_NULL`).                       |
| `u16ObjId_RESET`| UINT     | Object ID für den SoftKey "Reset" (Standard: `ID_NULL`).                     |
| `u16ObjId_TOGGLE`| UINT    | Object ID für den SoftKey "Toggle" (inkl. Green‑White‑Background) (Standard: `ID_NULL`). |
| `ID_SET_CALL`   | WSTRING  | Remote‑Methodenadresse (ACTION=CALL_METHOD) für den Set‑Aufruf auf Gerät B.  |
| `ID_RESET_CALL` | WSTRING  | Remote‑Methodenadresse (ACTION=CALL_METHOD) für den Reset‑Aufruf auf Gerät B.|
| `ID_TOGGLE_CALL`| WSTRING  | Remote‑Methodenadresse (ACTION=CALL_METHOD) für den Toggle‑Aufruf auf Gerät B.|
| `ID_STATE_READ` | WSTRING  | Lokal überwachte Adresse (BOOL, ACTION=READ) für den Flipflop‑Zustand, der von Gerät B remote beschrieben wird. |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

Keine externen Adapter an der SubApp‑Schnittstelle. Die Verdrahtung zu den internen Bausteinen erfolgt ausschließlich über Datenverbindungen und einen internen Adapter‑Socket.

## Funktionsweise

Die SubApp integriert zwei interne Bausteine:

1. **`SoftKeySRT_ASRT_AX`** (SubApp vom Typ `MyLib::sys::SoftKeySRT_ASRT_AX`):  
   Bündelt die HMI‑Logik für die drei SoftKeys (Set, Reset, Toggle) und das Green‑White‑Background. Sie stellt über einen **ASRT_AX‑Plug** eine standardisierte Schnittstelle nach außen bereit.

2. **`TRIGGER`** (FB vom Typ `adapter::net::ASRT_AX_CLIENT_0_SUBSCRIBE_1`):  
   Implementiert den OPC‑UA‑Client‑Teil. Er verbindet sich mit Gerät B, überwacht den Zustand des Flipflops (über `ID_STATE_READ`) und führt bei Bedarf die Remote‑Methodenaufrufe für Set, Reset und Toggle aus (über `ID_SET_CALL`, `ID_RESET_CALL`, `ID_TOGGLE_CALL`).

Die Verbindung zwischen HMI und Trigger wird über den **ASRT_AX**‑Adapter hergestellt: Der Plug von `SoftKeySRT_ASRT_AX` (Ausgang `OUT`) wird mit dem Socket `S_R_T` des `TRIGGER`‑FBs verbunden. Dadurch werden die SoftKey‑Ereignisse direkt an die OPC‑UA‑Trigger‑Logik weitergeleitet.

Die Objekt‑IDs (`u16ObjId_SET`, `u16ObjId_RESET`, `u16ObjId_TOGGLE`) werden über Datenverbindungen an `SoftKeySRT_ASRT_AX` übergeben und dort zur Identifikation der jeweiligen SoftKeys im HMI verwendet. Die Methodenadressen (`ID_*_CALL`) sowie die Zustandsadresse (`ID_STATE_READ`) werden an den `TRIGGER`‑FB übergeben und dort zur Kommunikation mit Gerät B genutzt.

## Technische Besonderheiten

- **Trennung von HMI und OPC‑UA‑Trigger**: Die SubApp ist eine Weiterentwicklung des Bausteins `Softkey_SRT_RPC_TO_Remote_BG_OPC`, bei dem diese Funktionalitäten nicht getrennt waren. Diese Entkopplung erleichtert Wartung und Test.
- **Verwendung des ASRT_AX‑Adapterkonzepts**: Der interne Plug/Socket‑Mechanismus gewährleistet eine klare Schnittstelle zwischen den Bausteinen und ermöglicht eine hohe Wiederverwendbarkeit.
- **Standardisierte OPC‑UA‑Kommunikation**: Der `TRIGGER`‑FB basiert auf dem Typ `ASRT_AX_CLIENT_0_SUBSCRIBE_1`, der einen OPC‑UA‑Client mit Subscribe‑Funktionalität darstellt. Die Adressen für Remote‑Methoden und Variablen sind als WSTRING konfigurierbar.
- **Initialwert `ID_NULL`**: Die Objekt‑IDs sind voreingestellt auf `ID_NULL`, was bedeutet, dass sie erst vor Inbetriebnahme gesetzt werden müssen, um einen gültigen SoftKey zu referenzieren.
- **Compiler‑Informationen**: Die SubApp ist im Paket `MyLib::sys` eingebettet und importiert den globalen Konstanten‑Wert `ID_NULL`.

## Zustandsübersicht

Die SubApp selbst besitzt keinen eigenen Zustandsautomaten. Die internen Bausteine `SoftKeySRT_ASRT_AX` und `TRIGGER` können jedoch eigene Zustände aufweisen:

- `SoftKeySRT_ASRT_AX` realisiert die Zustände der SoftKeys (z. B. „gedrückt“/„nicht gedrückt“) und das Ein/Aus‑Verhalten des Green‑White‑Backgrounds.
- Der `TRIGGER`‑FB verwaltet den Verbindungsstatus zum OPC‑UA‑Server und den Ablauf der Remote‑Methodenaufrufe.

Diese Zustände sind nach außen nicht direkt sichtbar, beeinflussen aber das Verhalten der SubApp, insbesondere die Ausführung der Remote‑Aufrufe.

## Anwendungsszenarien

Typische Einsatzfälle sind:

- **Bediengeräte in der Automatisierung**: Ein lokales HMI‑Panel (z. B. an einer Station) steuert über OPC‑UA Funktionen an einer entfernten Maschine oder Anlage.
- **Fernwartung und -diagnose**: SoftKeys werden verwendet, um z. B. Maschinenzustände (Set, Reset, Toggle) aus der Ferne zu ändern und den aktuellen Flipflop‑Status zu überwachen.
- **Flexible Systemarchitekturen**: Durch die Trennung von HMI‑Logik und Netzwerk‑Trigger kann die HMI unabhängig vom Kommunikationsprotokoll entwickelt oder ausgetauscht werden.

## Vergleich mit ähnlichen Bausteinen

Der Baustein `Softkey_SRT_RPC_TO_Remote_BG_OPC` stellt die ungetrennte Variante dar. Bei diesem sind HMI‑Logik und OPC‑UA‑Trigger direkt gekoppelt. Der `Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter` trennt diese beiden Aspekte durch die Einführung einer separaten `SoftKeySRT_ASRT_AX`‑SubApp und eines dedizierten `TRIGGER`‑FBs mit Adapterschnittstelle. Vorteile dieser Trennung:

- Bessere Testbarkeit und Modularität.
- Austauschbarkeit der OPC‑UA‑Implementierung ohne Änderung der HMI‑Logik.
- Wiederverwendung der HMI‑SubApp in anderen Kontexten (z. B. mit anderen Kommunikationsprotokollen).

Nachteilig ist der etwas höhere Verdrahtungsaufwand durch die zusätzliche Adapter‑Verbindung.

## Fazit

Der `Softkey_SRT_RPC_TO_Remote_BG_OPC_Adapter` ist ein leistungsfähiger Baustein für die Anbindung einer HMI an ein entferntes OPC‑UA‑Gerät. Durch die klare Trennung der Verantwortlichkeiten (HMI‑Logik vs. Kommunikation) bietet er eine hohe Flexibilität und erleichtert Integration sowie Wartung. Die Verwendung standardisierter Adapter (ASRT_AX) und konfigurierbarer Adressen macht ihn für vielfältige Automatisierungsszenarien geeignet.
