# A2X2_CLIENT_2_0_SUBSCRIBE_2

![A2X2_CLIENT_2_0_SUBSCRIBE_2](./A2X2_CLIENT_2_0_SUBSCRIBE_2.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock `A2X2_CLIENT_2_0_SUBSCRIBE_2` realisiert eine bidirektionale Datenübertragung über OPC-UA zwischen einem A2X2-Adapter und zwei externen OPC-UA-Knoten. Er kombiniert einen OPC-UA-Client (`CLIENT_2_0`) zum Schreiben von zwei Bool-Werten mit einem OPC-UA-Subscriber (`SUBSCRIBE_2`) zum Lesen von zwei Bool-Werten. Die Werte werden über `E_D_FF`-Flipflops gepuffert, um eine entkoppelte und zuverlässige Kommunikation zu gewährleisten. Der Baustein eignet sich insbesondere für Anwendungen, bei denen ein Steuerungssystem mit einer OPC-UA-basierten Leitebene kommunizieren muss und dabei sowohl Schreib- als auch Leseoperationen auf einfache Weise gebündelt werden sollen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
| Name | Typ | Kommentar |
|------|-----|-----------|
| `INIT` | EInit | Initialisierungsereignis; startet die Initialisierung des Subscribers und anschließend des Clients. |

### **Ereignis-Ausgänge**
| Name | Typ | Kommentar |
|------|-----|-----------|
| `INITO` | EInit | Bestätigung der erfolgreichen Initialisierung beider internen Bausteine. |
| `CNF` | Event | Wird ausgelöst, wenn sowohl der Write-Client als auch der Read-Subscriber eine Bestätigung liefern (QO = TRUE). |

### **Daten-Eingänge**
| Name | Typ | Kommentar |
|------|-----|-----------|
| `QI` | BOOL | Aktiviert die Kommunikation (TRUE = aktiv). Wird an beide internen Bausteine weitergegeben. |
| `ID_WRITE` | WSTRING | Remote-Zieladresse (ACTION=WRITE) für das Schreiben der beiden BOOL-Werte. |
| `ID_READ` | WSTRING | Lokal überwachter Zustandsknoten (ACTION=READ) für das Lesen der beiden BOOL-Werte. |

### **Daten-Ausgänge**
| Name | Typ | Kommentar |
|------|-----|-----------|
| `QO` | BOOL | TRUE nur wenn sowohl der Write-Client als auch der Read-Subscriber aktuell `QO = TRUE` melden. |
| `STATUS_WRITE` | WSTRING | Statusmeldung des internen `CLIENT_2_0`-Bausteins. |
| `STATUS_READ` | WSTRING | Statusmeldung des internen `SUBSCRIBE_2`-Bausteins. |

### **Adapter**
| Name | Typ | Kommentar |
|------|-----|-----------|
| `IO` | `adapter::types::bidirectional::A2X2` (Socket) | Bidirektionaler A2X2-Adapter: vier Datenleitungen (`DO_UP`, `DO_DOWN`, `DI_UP`, `DI_DOWN`) und vier Ereignisleitungen (`EI_UP`, `EI_DOWN`, `EO_UP`, `EO_DOWN`) für den Austausch von Bool-Werten. |

## Funktionsweise

Der Funktionsblock verbindet einen OPC-UA-Client und einen OPC-UA-Subscriber mit einem A2X2-Adapter. Die internen Abläufe lassen sich in zwei getrennte Datenpfade unterteilen:

**Schreibpfad (TX):**  
- Die über den Adapter eingehenden Datenwerte `IO.DO_UP` und `IO.DO_DOWN` werden durch die Ereignisse `IO.EO_UP` und `IO.EO_DOWN` in die jeweiligen `E_D_FF_TX_*`-Flipflops übernommen.  
- Die Ausgänge dieser Flipflops (`Q`) werden an die Daten-Eingänge `SD_1` und `SD_2` des Clients (`WRITE_CLIENT`) gelegt.  
- Jedes eingehende Ereignis am Adapter triggert den Client-Baustein (`REQ`), der die aktuellen Werte an die konfigurierte OPC-UA-Adresse (`ID_WRITE`) sendet.

**Lesepfad (RX):**  
- Der Subscriber (`READ_SUBSCRIBE`) empfängt kontinuierlich die beiden Bool-Werte von der OPC-UA-Adresse (`ID_READ`) über seine Daten-Ausgänge `RD_1` und `RD_2`.  
- Diese Werte werden bei jedem empfangenen Ereignis (`IND`) in die `E_D_FF_RX_*`-Flipflops übernommen.  
- Die Ausgänge dieser Flipflops speisen die Adapter-Datenausgänge `IO.DI_UP` und `IO.DI_DOWN` und werden über die Ereignisse `IO.EI_UP` und `IO.EI_DOWN` an den Adapter signalisiert.

**Initialisierung:**  
- Der `INIT`-Eingang initialisiert zuerst den Subscriber (`READ_SUBSCRIBE.INIT`).  
- Nach erfolgreicher Initialisierung des Subscribers (Ereignis `INITO`) wird der Client initialisiert (`WRITE_CLIENT.INIT`).  
- Der Client bestätigt die Initialisierung mit `INITO`, woraufhin der Baustein das Ereignis `INITO` nach außen abgibt.

**Fehler- und Statusüberwachung:**  
- Die Ereignisse `WRITE_CLIENT.CNF` und `READ_SUBSCRIBE.IND` werden gemeinsam auf den Eingang `REQ` des UND-Glieds `AND_QO` geführt.  
- Das UND-Glied setzt `QO` nur dann auf TRUE, wenn sowohl der Client als auch der Subscriber ihren Betriebszustand als aktiv melden (beide `QO = TRUE`).  
- Der Ausgang des UND-Glieds erzeugt das Ereignis `CNF`, um den Aufrufer über den aktuellen Zustand zu informieren.  
- Die Statuswerte des Clients und des Subscribers werden direkt auf `STATUS_WRITE` und `STATUS_READ` ausgegeben.

## Technische Besonderheiten

- **Bidirektionalität:** Der Adapter `A2X2` unterstützt sowohl das Senden als auch das Empfangen von Bool-Werten, was eine symmetrische Kommunikation ermöglicht.
- **OPC-UA-Integration:** Der Baustein nutzt die standardisierten 4diac-Bausteine `CLIENT_2_0` und `SUBSCRIBE_2` für die OPC-UA-Kommunikation. `ID_WRITE` und `ID_READ` müssen als vollständige OPC-UA-Knotenadressen (z.B. `ns=2;s=MyNode`) angegeben werden.
- **Pufferung:** Die `E_D_FF`-Flipflops entkoppeln die asynchronen Ereignisse der Adapter- und OPC-UA-Seite. Dadurch werden kurze Signalimpulse zuverlässig zwischengespeichert und spätere Datenänderungen korrekt weitergegeben.
- **Kombinierte Statusauswertung:** `QO` ist nur aktiv, wenn beide Kommunikationspfade (Senden und Empfangen) gleichzeitig in Ordnung sind. Dies ermöglicht eine einfache Überwachung der Gesamtverbindung.
- **Reihenfolge der Initialisierung:** Die Initialisierung erfolgt strikt sequenziell (erst Subscriber, dann Client). Dies stellt sicher, dass die Verbindung zum OPC-UA-Server erst nach erfolgreichem Abonnement aufgebaut wird.

## Zustandsübersicht

Der Funktionsblock besitzt keine expliziten Zustandsautomaten, da er vollständig aus konfigurierten Standardbausteinen besteht. Dennoch lassen sich logische Betriebszustände ableiten:

| Zustand | Beschreibung |
|---------|--------------|
| **Initialisierung** | Nach einem `INIT`-Impuls wird der Subscriber initialisiert, danach der Client. Nach erfolgreichem Abschluss wird `INITO` ausgegeben. |
| **Aktiv / Normalbetrieb** | `QI = TRUE`, Client und Subscriber sind erfolgreich verbunden. `QO` wird TRUE, wenn beide Bausteine ihre Bereitschaft signalisieren. |
| **Fehler / Nicht bereit** | Mindestens einer der internen Bausteine meldet `QO = FALSE` oder einen Status ungleich `"Idle"`. `QO` wird FALSE, `STATUS_WRITE` bzw. `STATUS_READ` enthalten die Fehlerdetails. |
| **Deaktiviert** | Bei `QI = FALSE` werden beide internen Bausteine deaktiviert; es werden keine Daten mehr übertragen. |

## Anwendungsszenarien

- **Fernsteuerung von Binärsignalen:** Ein Anlagensteuerungs-PLC sendet zwei Schaltzustände (z.B. Ventil auf/zu) und empfängt zwei Rückmeldungen über eine OPC-UA-Verbindung. Der Baustein bündelt beide Richtungen in einer einzigen Komponente.
- **Überwachung von Feldgeräten:** Ein Feldgerät mit A2X2-Schnittstelle wird über OPC-UA angebunden. Die vom Gerät gelieferten Bool-Werte werden per Subscription gelesen und auf der Steuerung verfügbar gemacht; umgekehrt können Sollwerte an das Gerät gesendet werden.
- **Integration in Leitsysteme:** Da `CLIENT_2_0` und `SUBSCRIBE_2` standardisierte 4diac-Bausteine sind, lässt sich der FB nahtlos in bestehende OPC-UA-basierte Automatisierungsarchitekturen integrieren.

## Vergleich mit ähnlichen Bausteinen

Ähnliche Bausteine, die nur eine Kommunikationsrichtung abdecken, sind z.B. `CLIENT_2_0` (nur Schreiben) oder `SUBSCRIBE_2` (nur Lesen). Der Kombinationsbaustein `A2X2_CLIENT_2_0_SUBSCRIBE_2` vereint beide Funktionen und bietet zusätzlich eine konfigurierte Pufferung sowie eine aggregierte Statusanzeige. Gegenüber einer manuellen Verschaltung der Standardbausteine spart er Entwicklungszeit und reduziert die Fehleranfälligkeit durch eine klar definierte Schnittstelle.

## Fazit

Der Funktionsblock `A2X2_CLIENT_2_0_SUBSCRIBE_2` ist eine wiederverwendbare, kompakte Lösung für die bidirektionale OPC-UA-Kommunikation mit einem A2X2-Adapter. Durch die Kombination von Client und Subscriber, die Pufferung der Datenpfade und die gemeinsame Statusüberwachung bietet er eine robuste und leicht integrierbare Komponente für Automatisierungsprojekte. Die strikte Trennung von Schreib- und Lesepfad sowie die klare Initialisierungsreihenfolge machen ihn sowohl für einfache Steuerungsaufgaben als auch für komplexere Leitsystemanbindungen geeignet.