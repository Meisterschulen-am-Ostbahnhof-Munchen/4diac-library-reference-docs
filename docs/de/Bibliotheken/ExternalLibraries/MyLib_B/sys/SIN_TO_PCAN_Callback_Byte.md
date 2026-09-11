# SIN_TO_PCAN_Callback_Byte


![SIN_TO_PCAN_Callback_Byte_network](./SIN_TO_PCAN_Callback_Byte_network.svg)

![SIN_TO_PCAN_Callback_Byte](./SIN_TO_PCAN_Callback_Byte.svg)

* * * * * * * * * *
## Einleitung
Der Funktionsbaustein **SIN_TO_PCAN_Callback_Byte** erzeugt ein sinusförmiges Signal und sendet dessen aktuellen Wert als CAN-Botschaft über eine PCAN-Callback-Schnittstelle. Er ist als Diagnose- und Debug-Baustein konzipiert, um Messwerte direkt in einem PCAN-Explorer darzustellen. Der erzeugte Sinuswert wird byte-genau in ein USINT-Format umgewandelt und als erstes Byte einer 8-Byte-Nachricht übertragen.

## Schnittstellenstruktur
Die Subapp besitzt ausschließlich eine **Adapter-Schnittstelle** als externe Anbindung. Es existieren keine separaten Ereignis- oder Datenein-/ausgänge – die gesamte Kommunikation erfolgt über den Adapter.

### **Ereignis-Eingänge**
– Keine

### **Ereignis-Ausgänge**
– Keine

### **Daten-Eingänge**
– Keine

### **Daten-Ausgänge**
– Keine

### **Adapter**
| Name | Typ | Richtung |
|------|-----|----------|
| `PLUG1` | `isobus::pgn::tx::Callback` | Ausgang (Plug) |

Der Adapter stellt eine Callback-Schnittstelle für das Senden von CAN-Nachrichten zur Verfügung. Über ihn wird das Senden ausgelöst und die Nachricht an die externe Umgebung übergeben.

## Funktionsweise
Der Baustein arbeitet ereignisgesteuert. Ein externes Triggerereignis (über den Adapter) startet den Ablauf:

1. **Ereignisauslösung** → `CallbackFB.REQ` aktiviert den Sinusgenerator `GEN_SIN`.
2. **Signalgenerierung** → `GEN_SIN` berechnet einen neuen Sinuswert basierend auf den Parametern (Amplitude, Offset, Periodendauer).
3. **Konvertierung** → Der LREAL-Wert wird zuerst in `USINT` umgewandelt (`F_LREAL_TO_USINT`) und danach in ein Byte (`F_USINT_TO_BYTE`).
4. **Byte-Array-Aufbau** → `BYTES_TO_ARR08B` setzt das erhaltene Byte an Position 0 eines 8-Byte-Arrays; die restlichen Bytes bleiben auf `16#00`.
5. **CAN-Nachricht erzeugen** → `STRUCT_MUX` packt das Byte-Array in eine CAN_MSG-Struktur (Priorität `7`, Datengröße `0`).
6. **Senden** → Der `CallbackFB` sendet die fertige Nachricht über den Adapter `PLUG1` nach außen.

Der Zyklus wiederholt sich bei jedem neuen Ereignis, sodass der Sinuswert kontinuierlich abgetastet und gesendet wird.

## Technische Besonderheiten
- **Byte-genaue Übertragung:** Der Sinuswert wird über `F_LREAL_TO_USINT` und `F_USINT_TO_BYTE` exakt in ein einzelnes Byte umgewandelt und im ersten Byte der CAN-Nachricht platziert.
- **Einsatz eines Callback-Adapters:** Die Subapp nutzt ausschließlich eine Adapter-Schnittstelle, was eine flexible Einbindung in bestehende PCAN-Kommunikationssysteme ermöglicht.
- **Konfigurierbare Signalparameter:** Der Sinusgenerator besitzt Parameter für Amplitude (`AM=10.0`), Offset (`OS=5.0`), Periodendauer (`PT=10s`) und Verzögerung (`DL=0.0`). Dadurch kann das Signal an verschiedene Anforderungen angepasst werden.
- **Direkte Strukturerzeugung:** Über `STRUCT_MUX` wird die CAN-Nachricht als strukturierter Datentyp `CAN_MSG` mit vordefinierter Priorität und Datengröße erzeugt.
- **Debug-/Plot-Eignung:** Die Daten sind als rohe Bytes aufbereitet, sodass sie von Analysewerkzeugen wie PCAN-Explorer unmittelbar interpretiert werden können.

## Zustandsübersicht
Der Baustein besitzt keinen expliziten Zustandsautomaten. Er arbeitet rein ereignisgesteuert: Jedes eingehende Ereignis am `CallbackFB.REQ` (über den Adapter) führt zu einem vollständigen Durchlauf der Signalverarbeitungskette und anschließendem Senden der Nachricht. Es gibt keine internen Wartezustände oder Verzögerungen außer der vom Sinusgenerator vorgegebenen Periodendauer (der jedoch nur bei jedem Trigger einen neuen Wert liefert).

## Anwendungsszenarien
- **Diagnose & Debug:** Darstellung eines kontinuierlich generierten Sinus-Signals in PCAN-Explorer zur Überwachung der CAN-Kommunikation.
- **Test von CAN-Knoten:** Erzeugen eines definierten Signalverlaufs zum Prüfen von Empfängern oder der Buslast.
- **Lehre und Ausbildung:** Demonstration der Verarbeitung eines Analogsignals zu einer CAN-Nachricht unter Verwendung standardisierter Umwandlungs- und Strukturierungsblöcke.

## Vergleich mit ähnlichen Bausteinen
Es existieren andere Signalgenerator-Bausteine, die direkt CAN-Nachrichten senden (z.B. mit integrierter ID- oder DLC-Verwaltung). Der vorliegende Baustein zeichnet sich jedoch durch folgende Besonderheiten aus:
- **Fokussierung auf ein einzelnes Datenbyte** – reduziert die Komplexität und eignet sich für schmale Signalwerte.
- **Adapterbasiertes Interface** – erlaubt eine einfache Integration in Systeme, die bereits einen Callback-Mechanismus verwenden.
- **Kombination aus Standard-Blöcken** (GEN_SIN, Konvertierung, Array-Bildung) ermöglicht eine transparente und erweiterbare Struktur, die leicht an andere Signaltypen angepasst werden kann.

## Fazit
Der Funktionsbaustein **SIN_TO_PCAN_Callback_Byte** stellt eine kompakte und effiziente Lösung dar, um ein analoges Sinus-Signal als CAN-Botschaft mit minimalem Aufwand zu senden. Durch die klare Trennung von Signalgenerierung, Konvertierung und CAN-Aufbereitung ist er gut verständlich, erweiterbar und besonders für diagnostische Zwecke geeignet. Die ausschließliche Verwendung einer Adapter-Schnittstelle macht ihn flexibel einsetzbar und erleichtert die Integration in bestehende Systeme der Automatisierungstechnik.