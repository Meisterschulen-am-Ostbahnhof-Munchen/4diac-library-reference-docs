# Q_ActiveMask_AUI

![Q_ActiveMask_AUI](./Q_ActiveMask_AUI.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **Q_ActiveMask_AUI** ist ein AUI‑Adapter‑Wrapper für den Baustein **Q_ActiveMask**. Er ermöglicht das Umschalten der aktiven Maske (Active Mask) in einem ISOBUS‑System (ISO 11783‑6) über eine unidirektionale AUI‑Schnittstelle. Der Baustein kapselt die Kommunikation mit dem internen `Q_ActiveMask`‑Kern und bietet eine einfache, adapterbasierte Anbindung für Ereignis‑ und Datenaustausch.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name  | Typ   | Kommentar |
|-------|-------|-----------|
| `INIT` | `EInit` | Service‑Initialisierung |

### **Ereignis-Ausgänge**

| Name  | Typ     | Kommentar |
|-------|---------|-----------|
| `INITO` | `EInit` | Bestätigung der Initialisierung |
| `CNF`   | `Event` | Bestätigung des angeforderten Dienstes |

### **Daten-Eingänge**

Keine direkten Dateneingänge vorhanden – alle Eingangsdaten werden über die AUI‑Adapter‑Sockets bereitgestellt.

### **Daten-Ausgänge**

| Name       | Typ     | Kommentar |
|------------|---------|-----------|
| `STATUS`   | `STRING`| Dienststatus |
| `s16result`| `INT`   | Rückgabewert (siehe Beschreibung) |

### **Adapter**

| Typ    | Name          | Richtung | Kommentar |
|--------|---------------|----------|-----------|
| `adapter::types::unidirectional::AUI` | `u16OldMaskId` | Plug (Ausgang) | Alte aktive Masken‑ID |
| `adapter::types::unidirectional::AUI` | `u16NewMaskId` | Socket (Eingang) | Neue aktive Masken‑ID |

## Funktionsweise

Der Baustein verbindet intern den eigentlichen Funktionsblock `Q_ActiveMask` (*isobus::UT::Q::Q_ActiveMask*) mit den Adapter‑Schnittstellen.  
Beim Eintreffen eines Ereignisses am Socket `u16NewMaskId.E1` wird das Ereignis `REQ` des internen FBs ausgelöst, wodurch der Befehl zum Ändern der aktiven Maske initiiert wird. Der interne FB verarbeitet den Befehl und sendet anschließend über `CNF` eine Bestätigung zurück, die über den Plug `u16OldMaskId.E1` an den verbundenen Empfänger weitergegeben wird.  
Die Nutzdaten (Masken‑IDs) werden über die Datenkanäle `D1` der AUI‑Adapter übertragen – `u16NewMaskId.D1` liefert die neue ID an den internen FB, während `u16OldMaskId.D1` die aktuelle (oder alte) ID ausgibt.  
Zusätzlich werden die Ergebnis‑Ausgänge `STATUS` und `s16result` direkt an die äußeren Ausgangsvariablen durchgeschliffen.

## Technische Besonderheiten

- **Adapter‑Wrapper:** Der Baustein besitzt keine eigenen Daten‑Eingänge; sämtliche Eingabedaten werden ausschließlich über die AUI‑Sockets empfangen.
- **Unidirektionale AUI‑Kommunikation:** Die verwendeten Adapter sind unidirektional – die Daten fließen nur in eine Richtung, was die Schnittstelle einfach und robust macht.
- **ISOBUS‑Konformität:** Der FB ist gemäß ISO 11783‑6 (Teil 6 – Anzeige und Bedienung) als Befehl zum Wechseln der aktiven Maske spezifiziert.
- **Keine Zustandsautomaten‑Logik im Wrapper:** Der Wrapper selbst enthält keine eigene Zustandsverwaltung, sondern delegiert alle Aktionen an den inneren FB `Q_ActiveMask`.

## Zustandsübersicht

Der Baustein selbst implementiert keinen expliziten Zustandsautomaten. Das Verhalten wird durch den inneren FB `Q_ActiveMask` bestimmt.  
Hinsichtlich der Ereignis‑Steuerung lässt sich folgender Ablauf beschreiben:

1. **Initialisierung** – Nach dem Ereignis `INIT` wird der interne FB initialisiert; die erfolgreiche Initialisierung wird über `INITO` gemeldet.
2. **Befehlsausführung** – Nach Erhalt eines Ereignisses am Socket `u16NewMaskId.E1` wird der Befehl zur Maskenänderung ausgelöst.
3. **Bestätigung** – Sobald die Ausführung abgeschlossen ist, wird das Ereignis `CNF` ausgegeben und die Ergebnisdaten (`STATUS`, `s16result`) stehen zur Verfügung.

## Anwendungsszenarien

- Einsatz in ISOBUS‑Terminals oder Steuergeräten, um die aktuell angezeigte Maske (Display‑Seite) dynamisch zu wechseln.
- Einbindung in größere Applikationen, bei denen Maskenwechsel über standardisierte AUI‑Schnittstellen erfolgen sollen, z. B. bei der Umsetzung von Bedienabläufen nach ISO 11783‑6.
- Als Adapter‑Wrapper für Systeme, die eine einheitliche, unidirektionale Kommunikation für Maskensteuerung verwenden.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zum direkten Funktionsblock `Q_ActiveMask` bietet **Q_ActiveMask_AUI** eine zusätzliche Abstraktionsschicht über AUI‑Adapter. Dadurch wird die Integration in adapterbasierte Architekturen erleichtert, während die eigentliche Befehlslogik unverändert bleibt. Der direkte FB erwartet separate Ereignis‑ und Datenleitungen; der Wrapper bündelt diese in einer unidirektionalen Adapter‑Schnittstelle und reduziert so die Verdrahtungskomplexität.

## Fazit

Der Funktionsblock **Q_ActiveMask_AUI** ist ein kompakter, adapterbasierter Wrapper zur Steuerung der aktiven Maske in ISOBUS‑Anwendungen. Er abstrahiert die komplexe Ein‑/Ausgabe‑Schnittstelle des zugrunde liegenden FB `Q_ActiveMask` und ermöglicht eine einfache, standardisierte Anbindung über unidirektionale AUI‑Adapter. Dadurch eignet er sich besonders für modulare Systeme, die eine klare Trennung von Ereignis‑ und Datenfluss anstreben.
