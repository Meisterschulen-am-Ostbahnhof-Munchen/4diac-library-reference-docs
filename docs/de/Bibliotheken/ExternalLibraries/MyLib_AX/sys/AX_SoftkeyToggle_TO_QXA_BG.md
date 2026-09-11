# AX_SoftkeyToggle_TO_QXA_BG


![AX_SoftkeyToggle_TO_QXA_BG_network](./AX_SoftkeyToggle_TO_QXA_BG_network.svg)

![AX_SoftkeyToggle_TO_QXA_BG](./AX_SoftkeyToggle_TO_QXA_BG.svg)

* * * * * * * * * *
## Einleitung

Die SubApp **AX_SoftkeyToggle_TO_QXA_BG** realisiert eine Softkey-basierte Toggle-Funktion mit visueller Zustandsrückmeldung. Ein Tastendruck beziehungsweise ein definiertes Softkey-Release-Ereignis kippt ein internes Toggle-Flip-Flop und schaltet darüber einen logiBUS-Ausgang (QXA). Parallel wird der Hintergrund eines verbundenen Anzeige-Bausteins an den aktuellen Schaltzustand angepasst. Die SubApp ist generisch aufgebaut und kann über zwei Eingangsparameter flexibel an unterschiedliche Softkeys und Ausgänge angepasst werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

| Name | Datentyp | Initialwert | Beschreibung |
|------|----------|-------------|--------------|
| `u16ObjId` | `UINT` | `ID_NULL` | Objekt-ID des zu überwachenden Softkeys. |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Identifiziert den physischen Ausgang (`Output_Q1` bis `Output_Q8`). |

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Die SubApp verarbeitet intern eine Softkey-Eingabe und setzt daraus einen stabilen binären Zustand:

1. Der Funktionsbaustein **Softkey_IE** überwacht den über `u16ObjId` spezifizierten Softkey. Er ist so konfiguriert, dass er auf das Ereignis **SK_RELEASED** (Loslassen der Taste) reagiert.
2. Bei einem erkannten Release erzeugt **Softkey_IE** ein Ereignis an seinem Ausgang `IND`.
3. Dieses Ereignis wird an den Takteingang `CLK` des Toggle-Flip-Flops **AX_T_FF** weitergeleitet.
4. Das Flip-Flop wechselt bei jedem Takt seinen Zustand `Q` zwischen `FALSE` und `TRUE`.
5. Der Adapter **AX_SPLIT_2** verteilt den Zustand auf zwei parallele Pfade:
   - `OUT1` führt zum Funktionbaustein **logiBUS_QXA**, der den physischen Ausgang entsprechend setzt.
   - `OUT2` führt zum SubApp-Baustein **GreenWhiteBackground_AX**, der die Hintergrundfarbe des Bedienelements an den Zustand anpasst.

Somit bewirkt jeder Softkey-Release einen Wechsel des Ausgangszustands und der dargestellten Hintergrundfarbe.

## Technische Besonderheiten

- Die SubApp besitzt ausschließlich Daten-Eingänge. Ereignisse und Adapter werden nur intern verwendet und sind nach außen nicht sichtbar.
- Die Parameter `u16ObjId` und `Output` sind als generische Eingänge ausgeführt. Dadurch kann der Baustein für beliebige Softkeys und Ausgänge verwendet werden, ohne die interne Struktur zu verändern.
- Die Initialwerte `ID_NULL` beziehungsweise `logiBUS_DO::Invalid` stellen sicher, dass der Baustein ohne explizite Konfiguration zunächst keine Aktion ausführt.
- Die Verbindungen der Eingänge zu den internen Funktionbausteinen sind im Editor als unsichtbar markiert (`Visible = false`), wodurch die Netzübersicht kompakt bleibt.
- Die Kopplung von Ausgangstreiber und Hintergrund-Feedback über einen gemeinsamen Adapter-Splitter reduziert den Verdrahtungsaufwand und gewährleistet einen synchronen Zustandswechsel.

## Zustandsübersicht

Das interne Toggle-Flip-Flop besitzt zwei stabile Zustände:

| Zustand des Flip-Flops | Bedeutung für den Ausgang | Hintergrundanzeige |
|------------------------|---------------------------|--------------------|
| `Q = FALSE` | Ausgang inaktiv | Weiß |
| `Q = TRUE` | Ausgang aktiv | Grün |

Jedes Softkey-Release-Ereignis erzeugt einen Wechsel zwischen diesen beiden Zuständen. Der aktuelle Zustand bleibt solange erhalten, bis das nächste Ereignis eintrifft.

## Anwendungsszenarien

- **Bedienfelder mit Softkeys**: Umschaltung zwischen zwei Betriebsarten oder Funktionen über einen einzelnen Softkey.
- **Visuelle Statusanzeige**: Der Hintergrund des Softkeys oder eines zugeordneten Elements zeigt den aktuellen Schaltzustand an.
- **EIN/AUS-Steuerung**: Ein Softkey dient als Taster für einen dauerhaft aktiven oder inaktiven Ausgang.
- **Generische Wiederverwendung**: Durch die konfigurierbaren Eingänge kann derselbe Baustein an verschiedenen Softkey-Positionen und Ausgängen eingesetzt werden.

## Vergleich mit ähnlichen Bausteinen

Gegenüber einem einzelnen Toggle-Flip-Flop wie **AX_T_FF** bietet dieser Baustein eine höhere Integration: Die Softkey-Erkennung, die Toggle-Logik, die Ausgangsansteuerung und die Hintergrundrückmeldung sind in einer wiederverwendbaren SubApp zusammengefasst. Im Unterschied zu einem einfachen Taster-Baustein, der nur während des Drückens aktiv ist, bleibt der Zustand hier über das Release-Ereignis hinweg erhalten. Die Kapselung erleichtert die Wartung und reduziert die Komplexität im übergeordneten Anwendungsnetz.

## Fazit

Die SubApp **AX_SoftkeyToggle_TO_QXA_BG** ist eine kompakte und flexibel einsetzbare Lösung für Softkey-basierte Umschaltaufgaben. Sie kombiniert Ereignisverarbeitung, Zustandsspeicherung, Ausgangsansteuerung und visuelles Feedback in einem einzigen, generisch konfigurierbaren Baustein. Durch die klare Trennung der internen Funktionen und die ausschließlich über Daten-Eingänge erfolgende Parametrierung eignet sie sich hervorragend für den Einsatz in unterschiedlichen Bedien- und Anzeigesystemen.