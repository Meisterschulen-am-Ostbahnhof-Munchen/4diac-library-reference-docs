# AE2_ILOCK_T_FF_TO_BOOL_EVENT


![AE2_ILOCK_T_FF_TO_BOOL_EVENT_network](./AE2_ILOCK_T_FF_TO_BOOL_EVENT_network.svg)

![AE2_ILOCK_T_FF_TO_BOOL_EVENT](./AE2_ILOCK_T_FF_TO_BOOL_EVENT.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **AE2_ILOCK_T_FF_TO_BOOL_EVENT** ist ein Kettenglied eines wechselseitig verriegelten Toggle-Flip-Flops. Er nutzt einen bidirektionalen **AE2-Adapter** als SOCKET/PLUG-Kette, wodurch beliebig viele Teilnehmer hintereinander geschaltet werden können. Der Ausgang wird als **BOOL Q** und als **Event EO** bereitgestellt.

Der Baustein ist generisch einsetzbar: Der PLUG eines Kettenglieds wird mit dem SOCKET des nächsten verbunden. Auf diese Weise entsteht eine wechselseitige Verriegelung, bei der genau ein Teilnehmer aktiv ist und den Zustand kontrolliert weitergibt.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ   | Beschreibung |
|------|-------|--------------|
| IND  | Event | Startet die Verriegelungs- und Weitergabelogik des Kettenglieds. |

### **Ereignis-Ausgänge**

| Name | Typ   | Beschreibung |
|------|-------|--------------|
| EO   | Event | Wird ausgelöst, wenn das interne SR-Flipflop seinen Zustand ändert. |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

| Name | Typ  | Beschreibung |
|------|------|--------------|
| Q    | BOOL | Aktueller Zustand des internen Flipflops. |

### **Adapter**

| Name    | Richtung | Typ                                   | Beschreibung |
|---------|----------|---------------------------------------|--------------|
| SOCKET  | Eingang  | `adapter::types::bidirectional::AE2`  | Empfängt Ereignisse vom vorherigen Kettenglied. |
| PLUG    | Ausgang  | `adapter::types::bidirectional::AE2`  | Sendet Ereignisse an das nächste Kettenglied. |

## Funktionsweise

Der Baustein besteht intern aus einem SR-Flipflop (`E_SR_I1`), einem Ereignis-Schalter (`E_SWITCH_I1`) sowie zwei AE2-Konvertern (`AE2_EVENT_TO_E` und `AE2_E_TO_EVENT`).

Ein eingehendes Ereignis an `IND` wird zunächst dem Ereignis-Schalter zugeführt. Dieser prüft den aktuellen Zustand von `Q`:

- Ist `Q = 1` (aktiv), wird der Pfad `EO0` aktiviert. Dieser setzt das SR-Flipflop über `S` und stößt gleichzeitig die Kommunikation über die AE2-Adapter an.
- Ist `Q = 0` (inaktiv), wird der Pfad `EO1` aktiviert. Dieser setzt das SR-Flipflop über `R` zurück, ohne die Adapterkette zu verwenden.

Die beiden AE2-Konverter sind so verschaltet, dass Ereignisse vom `SOCKET` zum `PLUG` und umgekehrt weitergereicht werden. Erst nach Abschluss dieser Übertragung wird das SR-Flipflop über die Rückkopplung (`CNF`) zurückgesetzt. Dadurch wird sichergestellt, dass die Weitergabe an das nächste Kettenglied vollständig abgeschlossen ist, bevor der eigene Zustand geändert wird.

Der Ausgang `Q` gibt den aktuellen Zustand des internen SR-Flipflops aus. Das Ereignis `EO` signalisiert eine Zustandsänderung und kann zum Beispiel zur Weiterverarbeitung in der Steuerung verwendet werden.

## Technische Besonderheiten

- **AE2-Bidirektional-Kette:** Durch die Kombination von `SOCKET` und `PLUG` ist eine einfache Verkettung mehrerer Instanzen möglich.
- **Handshake über AE2-Konverter:** Die Konverter `AE2_EVENT_TO_E` und `AE2_E_TO_EVENT` sind kreuzgekoppelt, sodass eine gesicherte Weitergabe über die Kette erfolgt.
- **Kombinierter Ausgang:** Der Zustand wird sowohl als BOOL (`Q`) als auch als Event (`EO`) bereitgestellt.
- **Wiederverwendbar:** Der Baustein ist als SubApp gekapselt und kann in verschiedenen Steuerungsprojekten mehrfach instanziiert werden.

## Zustandsübersicht

| Q (vor IND) | Zustand    | Reaktion auf IND                                                                                   |
|-------------|------------|-----------------------------------------------------------------------------------------------------|
| `0`         | inaktiv    | `EO1` wird aktiviert, das SR-Flipflop wird zurückgesetzt, `Q` bleibt `0`. Keine Adapteraktivität.  |
| `1`         | aktiv      | `EO0` wird aktiviert, das SR-Flipflop wird gesetzt und die Adapterkette wird angestoßen. Nach erfolgreicher Weitergabe wird `Q` auf `0` zurückgesetzt. |

## Anwendungsszenarien

- **Rundlaufsteuerung:** Mehrere Teilnehmer werden zu einer Kette verbunden; genau ein Teilnehmer ist aktiv und gibt den Aktivitätszustand an den nächsten weiter.
- **Wechselseitige Verriegelung:** Maschinen oder Anlagen, bei denen nie zwei Teilnehmer gleichzeitig aktiv sein dürfen.
- **Erweiterte Ausgabe:** Wenn neben der Adapter-Kommunikation auch ein BOOL-Signal und ein Ereignis für die übergeordnete Steuerung benötigt werden.

## Vergleich mit ähnlichen Bausteinen

| Baustein                          | Unterschied |
|-----------------------------------|-------------|
| `AE2_ILOCK_T_FF_TO_BOOL_EVENT`   | Ausgang als `BOOL Q` und `Event EO`. |
| `MyLib::sys::AE2_ILOCK_T_FF_TO_AX` | Gleiche interne Logik, aber Ausgang als AX-Adapter anstelle von BOOL und Event. |
| Einfaches `E_SR` ohne Adapter      | Keine Kopplung mehrerer Bausteine über AE2, keine wechselseitige Verriegelung. |

## Fazit

`AE2_ILOCK_T_FF_TO_BOOL_EVENT` ist eine vielseitig einsetzbare SubApp für verriegelte Toggle-Ketten. Durch die Kombination aus AE2-Adapterkette, internem SR-Flipflop und BOOL/Event-Ausgang eignet er sich besonders für Steuerungsaufgaben, bei denen mehrere Teilnehmer wechselseitig gesperrt werden müssen und gleichzeitig ein direkter digitaler Zustand sowie ein Ereignis benötigt werden.