# CTU_TO_PCAN_Callback


![CTU_TO_PCAN_Callback_network](./CTU_TO_PCAN_Callback_network.svg)

![CTU_TO_PCAN_Callback](./CTU_TO_PCAN_Callback.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `CTU_TO_PCAN_Callback` ist eine Subapplikation (SubApp), die als Diagnose- und Debug-Baustein dient. Er zählt jedes Ereignis, das von einem PCAN-Callback ausgelöst wird, und sendet den aktuellen Zählerwert als CAN-Botschaft über den PCAN-Explorer aus. Dadurch können Ereignisse oder Triggerzustände auf dem CAN-Bus direkt visualisiert und analysiert werden.

Die SubApp kapselt die notwendige Verarbeitungskette: einen Aufwärtszähler (E_CTU), eine Konvertierung des Zählerstands in eine CAN-Nachricht und die Übertragung über den mitgelieferten Adapter. Sie wurde zur Wiederverwendung aus einer größeren Übung ausgelagert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine externen Ereignis-Eingänge vorhanden. Die SubApp wird ausschließlich über den Adapter angesteuert, der Ereignisse vom PCAN-Callback empfängt.

### **Ereignis-Ausgänge**

Keine externen Ereignis-Ausgänge vorhanden.

### **Daten-Eingänge**

Keine externen Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

Keine externen Daten-Ausgänge vorhanden.

### **Adapter**

- **PLUG1** (Typ: `isobus::pgn::tx::Callback`)  
  Dieser Adapter verbindet die SubApp mit dem PCAN-Callback-Sender. Über ihn werden die Callback-Ereignisse empfangen (REQ) und die CAN-Botschaften gesendet (über den internen CallbackFB).

## Funktionsweise

Die SubApp arbeitet ereignisgesteuert:

1. Über den Adapter `PLUG1` wird ein Callback-Ereignis empfangen (dies entspricht einem Trigger vom PCAN-System).
2. Das Ereignis wird an den internen Funktionsblock `CallbackFB` (vom Typ `isobus::pgn::tx::CallbackFB`) über die Verbindung `CallbackFB.REQ` weitergeleitet.
3. Gleichzeitig wird das Ereignis an den Zähler `E_CTU` (Ereignis-Eingang `CU`) gesendet, sodass dieser bei jedem Trigger um 1 erhöht wird.
4. Nach der Zählung gibt `E_CTU` den aktuellen Zählerstand (`CV`) als `UINT`‑Wert aus. Dieser wird über `F_UINT_TO_BYTE` in ein einzelnes Byte (0‑255) umgewandelt.
5. Das Byte wird in `BYTES_TO_ARR08B` in ein Byte‑Array (8 Bytes) eingefügt (an Position 0, die restlichen Bytes sind mit 0 initialisiert).
6. Das Byte‑Array wird über `STRUCT_MUX` in eine CAN-Nachrichtenstruktur (`isobus::pgn::CAN_MSG`) verpackt. Dabei werden zusätzlich die Parameter `u8Priority` (7) und `u16DaSize` (0) gesetzt.
7. Die fertige CAN-Nachricht wird über `STRUCT_MUX` an `CallbackFB.DI1` übergeben und durch das Ereignis `CallbackFB.CNF` (das nach erfolgreicher Verarbeitung ausgelöst wird) zurückbestätigt.
8. `CallbackFB` sendet die Nachricht über den Adapter `PLUG1` an das PCAN-System, wo sie im PCAN‑Explorer dargestellt werden kann.

Durch diese Kette wird bei jedem PCAN‑Callback der Zählerstand als CAN‑Botschaft mit der CAN‑ID und Priorität 7 gesendet.

## Technische Besonderheiten

- **Zählerbegrenzung**: Der Zähler `E_CTU` ist auf 0 als Startwert (`PV = 0`) gesetzt und zählt bei jedem Ereignis hoch. Da der Ausgang `CV` über `F_UINT_TO_BYTE` in ein Byte umgewandelt wird, ist der effektive Wertebereich auf 0–255 begrenzt. Nach 255 läuft der Zähler über.
- **Konvertierungskette**: Mehrere Konvertierungsbausteine (`F_UINT_TO_BYTE`, `BYTES_TO_ARR08B`, `STRUCT_MUX`) werden verwendet, um den Zählerstand in das Format einer CAN‑Nachricht zu bringen. Diese Kette ist erforderlich, weil die PCAN‑Callback‑Schnittstelle eine spezifische Datenstruktur (`CAN_MSG`) erwartet.
- **Adapter‑Kapselung**: Die SubApp besitzt ausschließlich einen Adapter als Schnittstelle. Dadurch ist sie vollständig in bestehende PCAN‑Callback‑Netzwerke integrierbar, ohne dass zusätzliche Ein‑/Ausgänge erforderlich sind.
- **Diagnosezweck**: Der Baustein ist für Debugging‑ und Plotting‑Anwendungen konzipiert, z. B. um Ereignisse oder Zustandsänderungen auf dem CAN‑Bus sichtbar zu machen.

## Zustandsübersicht

Der Baustein besitzt keine eigenen expliziten Zustände. Er arbeitet rein ereignisgesteuert:

- Ruhezustand: Kein Callback-Ereignis, keine Aktivität.
- Aktiver Zustand: Ein Callback-Ereignis tritt ein, der Zähler wird inkrementiert, die Nachricht wird zusammengesetzt und gesendet.
- Nach erfolgreichem Senden kehrt der Baustein in den Ruhezustand zurück.

Die interne Zustandslogik wird vollständig von den verwendeten Funktionsblöcken (`E_CTU`, `CallbackFB`) übernommen.

## Anwendungsszenarien

- **Debugging von CAN‑Kommunikation**: Überwachung, wie oft bestimmte CAN‑Botschaften oder Ereignisse auftreten (z. B. Zähler für Fehler‑ oder Statusmeldungen).
- **PCAN‑Explorer‑Plotting**: Darstellung des Zählerstands als kontinuierliche CAN‑Botschaft, um zeitliche Verläufe zu visualisieren.
- **Ereignisüberwachung**: Zählen von Callbacks, die von anderen Systemkomponenten ausgelöst werden, und Übertragung dieser Zählerwerte auf den CAN‑Bus.
- **Integration in Testumgebungen**: Als diagnostischer Zusatzbaustein in bestehenden PCAN‑Callback‑Anwendungen, ohne Änderungen an der Hauptlogik.

## Vergleich mit ähnlichen Bausteinen

Es gibt ähnliche Diagnose‑Bausteine, die Zählerstände senden, jedoch unterscheiden sich diese oft in der Art der Ereignisauslösung oder der Datenaufbereitung:

- **E_CTU direkt + manueller Sender**: Statt einer SubApp könnte man E_CTU und einen Sender getrennt verdrahten. Der Vorteil der SubApp ist die Wiederverwendbarkeit und klare Kapselung der Konvertierungskette.
- **Andere Callback‑Sender**: Bausteine, die nur CAN‑Nachrichten senden, ohne Zähler. Dieser Baustein integriert Zählung und Senden in einem Modul.
- **Bausteine mit fester CAN‑ID**: Im Gegensatz zu Bausteinen, bei denen die CAN‑ID konfiguriert werden muss, verwendet dieser Baustein die im CallbackFB definierte Konfiguration (hier als Parameter `DI1` vorgegeben – im Standardfall alle Bytes 0xFF).

Die SubApp zeichnet sich durch ihre einfache Integration, die automatische Zählfunktion und die vollständige Datenkonvertierung aus.

## Fazit

`CTU_TO_PCAN_Callback` ist ein kompakter und wiederverwendbarer Diagnosebaustein für PCAN‑Callback‑Anwendungen. Er kombiniert einen Zähler mit der Übertragung des Zählerstands als CAN‑Botschaft auf elegante Weise. Die SubApp ist ideal für Entwickler, die Ereigniszähler auf dem CAN‑Bus beobachten oder plotten möchten, ohne sich um die Details der Datenkonvertierung kümmern zu müssen. Dank der Adapter‑Schnittstelle lässt sie sich nahtlos in bestehende PCAN‑Callback‑Netzwerke integrieren und trägt zur besseren Debugging‑ und Überwachungsfähigkeit bei.
