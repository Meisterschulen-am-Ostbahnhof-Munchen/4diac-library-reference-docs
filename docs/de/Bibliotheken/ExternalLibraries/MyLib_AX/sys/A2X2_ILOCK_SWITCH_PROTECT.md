# A2X2_ILOCK_SWITCH_PROTECT


![A2X2_ILOCK_SWITCH_PROTECT_network](./A2X2_ILOCK_SWITCH_PROTECT_network.svg)

![A2X2_ILOCK_SWITCH_PROTECT](./A2X2_ILOCK_SWITCH_PROTECT.svg)

* * * * * * * * * *
## Einleitung
Der Funktionsblock **A2X2_ILOCK_SWITCH_PROTECT** ist eine Subapp, die einen bidirektionalen A2X2-Kommunikationskanal (z. B. für Tasterzustände und Rückschreibewerte) mit einer Last-Wins-Arbitrierung und einer optionalen Schutz-Totzeit kombiniert. Er kapselt die notwendigen Grundbausteine (Bridge, Interlock, Split) in einer wiederverwendbaren Composite-Struktur, sodass auf der Ressourcen-Ebene nur der gebündelte A2X2-Anschluss und der ausgehende A2X-Plug für physische Ausgänge sichtbar sind. Das ermöglicht eine saubere Trennung von Netzwerkkommunikation und lokaler Signalverarbeitung.

## Schnittstellenstruktur
### **Ereignis-Eingänge**
- Keine (Ereignissteuerung erfolgt über die internen Adapterverbindungen)

### **Ereignis-Ausgänge**
- Keine (das Zeitverhalten wird über den Adapter `timeOut` des internen ILOCK-Bausteins abgewickelt)

### **Daten-Eingänge**
| Name         | Typ  | Initialwert | Kommentar                                   |
|--------------|------|-------------|---------------------------------------------|
| `DT_PROTECT` | TIME | `T#50ms`    | Schutz-Totzeit vor einem Richtungswechsel   |

### **Daten-Ausgänge**
- Keine

### **Adapter**
| Richtung | Name | Typ                                        | Kommentar                                      |
|----------|------|--------------------------------------------|------------------------------------------------|
| Socket   | `IO` | `adapter::types::bidirectional::A2X2`      | Gebündelter NET-Roundtrip: Taster-Zustand rein, Rückschreibe-Zustand raus |
| Plug     | `OUT`| `adapter::types::unidirectional::A2X`      | Interlocked UP/DOWN-Zustand, für physische Ausgänge |

## Funktionsweise
Die Subapp verarbeitet den gesamten Datenfluss von einem bidirektionalen A2X2-Kanal zur lokalen Interlock-Logik und zurück. Die interne Topologie besteht aus vier Funktionsbausteinen:

1. **BRIDGE** (`A2X2_TO_A2X`): Zerlegt das bidirektionale A2X2-Signal in zwei unidirektionale Pfade – den hereinkommenden Tasterzustand (A2X_OUT) und den zurückschreibenden Zustand (A2X_IN).
2. **ILOCK** (`ILOCK_SWITCH_PROTECT_A2X`): Implementiert die Last-Wins-Arbitrierung. Es wertet den ankommenden Tasterzustand aus und erzeugt einen stabilen Ausgangszustand (UP/DOWN), wobei ein Richtungswechsel erst nach Ablauf der durch `DT_PROTECT` definierten Totzeit erlaubt wird. Ein interner Timeout-Adapter signalisiert das Ende der Totzeit.
3. **E_TimeOut** (Ereignis-FB `E_TimeOut`): Steuert das Zeitverhalten des ILOCK-Bausteins, indem es nach Empfang des `timeOut`-Signals eine vordefinierte Verzögerung (die Totzeit) abwartet und dann das entsprechende Ereignis auslöst.
4. **SPLIT** (`A2X_SPLIT_2`): Dupliziert den interlocked Zustand von `ILOCK.OUT` auf zwei Ausgänge:
   - `OUT1` → externe Schnittstelle `OUT` (für physische Aktoren)
   - `OUT2` → zurück zum `BRIDGE.A2X_IN`, um den Zustand über den A2X2-Roundtrip an das übergeordnete System zu senden.

Die Verbindung `ILOCK.timeOut` mit `E_TimeOut.TimeOutSocket` sorgt dafür, dass der Interlock nach Ablauf der Schutzzeit wieder für neue Richtungswechsel bereit ist.

## Technische Besonderheiten
- **Composite-Design**: Alle Bausteine sind innerhalb der Subapp gekapselt; die Ressource sieht nur die Adapter `IO` und `OUT`. Dadurch wird die Wiederverwendbarkeit erhöht und die Netzwerkkonfiguration vereinfacht.
- **Last-Wins-Arbitrierung**: Der zuletzt aktivierte Taster gewinnt. Eine schnelle Umkehr (z. B. versehentliches Entprellen) wird durch die Schutz-Totzeit verhindert.
- **Bidirektionale Kommunikation**: Über den A2X2-Socket werden nicht nur Tasterzustände empfangen, sondern auch Rückschreibewerte (z. B. für Statusanzeigen) gesendet. Der Rückschreibepfad wird vom SPLIT-Baustein direkt aus dem Interlock-Zustand abgeleitet.
- **Parametrierbare Totzeit**: `DT_PROTECT` ist als Eingang konfigurierbar und kann zur Laufzeit angepasst werden (Standard: 50 ms).
- **Ereignisgesteuerte Zeitsteuerung**: Die Totzeit wird über den Ereignisbaustein `E_TimeOut` realisiert, was eine präzise und deterministische Zeitbasis ermöglicht.

## Zustandsübersicht
Die Subapp selbst besitzt keine eigenen Zustände, da sie lediglich eine Komposition interner FB ist. Der interne ILOCK-Baustein hat typischerweise folgende Zustände:
- **IDLE**: Kein Taster aktiv, Ausgang neutral.
- **UP**: Der Aufwärts-Taster wurde zuletzt gedrückt (Ausgang high).
- **DOWN**: Der Abwärts-Taster wurde zuletzt gedrückt (Ausgang low).
- **PROTECT**: Ein Richtungswechsel wurde angefordert, aber die Totzeit läuft noch. Nach Ablauf von `DT_PROTECT` wird in den entsprechenden Zielzustand gewechselt.

Diese Zustände werden über den `OUT`-Adapter nach außen als A2X-Signal sichtbar.

## Anwendungsszenarien
- **Steuerung von Antrieben** (z. B. Hub-/Senkmechanismen) mit zwei Tastern (Auf/Ab) über ein Netzwerkprotokoll, wobei der letzte Tastendruck Priorität hat.
- **Sicherheitskritische Schaltungen**, bei denen ein Richtungswechsel eine kurze Verzögerung benötigt, um mechanische Schäden zu vermeiden (z. B. Pumpen, Ventile).
- **Dezentrale Automatisierungssysteme**, bei denen Taster an einer entfernten Station eingelesen und der endgültige Schaltzustand über den Rückschreibepfad an die Zentrale gemeldet wird.
- **Anlagen mit mehreren Bedienpunkten**, die denselben A2X2-Kanal nutzen und per Last-Wins-Logik koordiniert werden.

## Vergleich mit ähnlichen Bausteinen
- **Ohne Totzeit** (`ILOCK_SWITCH` o. ä.): Reagieren sofort auf Richtungswechsel, können aber bei Prellen oder Rauschen zu unerwünschten Schaltvorgängen führen. `A2X2_ILOCK_SWITCH_PROTECT` fügt eine Schutzzeit ein.
- **Nur unidirektional** (`A2X_ILOCK...`): Verwenden einen separaten Rückkanal für den Rückschreibewert, während dieser FB den bidirektionalen A2X2-Kanal integriert.
- **Getrennte Bausteine**: Man könnte Bridge, Interlock und Split separat verwenden, müsste jedoch die Verbindungen selbst aufbauen und hätte eine komplexere Ressourcen-Konfiguration. Diese Subapp bietet eine vorgefertigte, kompakte Lösung.

## Fazit
`A2X2_ILOCK_SWITCH_PROTECT` ist ein vielseitiger, in sich geschlossener Funktionsbaustein für Anwendungen, die eine robuste Taster-Interlock mit Totzeit übeir ein bidirektionales Netzwerkprotokoll benötigen. Die Kombination aus Bridge, Interlock und Split ermöglicht eine einfache Einbindung in bestehende Systeme, während die letztliche Arbitrierung zuverlässig und sicher arbeitet. Durch die Parametrierbarkeit der Schutzzeit lässt sich der Baustein flexibel an unterschiedliche Anforderungen anpassen.