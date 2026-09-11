# SINT_TO_Q_NumericValue


![SINT_TO_Q_NumericValue_network](./SINT_TO_Q_NumericValue_network.svg)

![SINT_TO_Q_NumericValue](./SINT_TO_Q_NumericValue.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **SINT_TO_Q_NumericValue** ist eine wiederverwendbare Subapplikation, die einen SINT-Wert (z. B. eine Schrittkettennummer) auf einem Bediengerät (VT) in einem Zahlenfeld anzeigt. Die Objekt-ID des Ziel-Zahlenfeldes ist parametrierbar und wird über den Eingang `u16ObjId` zugeführt. Intern erfolgt eine Typkonvertierung von SINT nach UINT, um die Anforderungen des verwendeten Anzeigebausteins `Q_NumericValue` zu erfüllen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Datentyp | Beschreibung |
|------|----------|--------------|
| `CNF` | Event | Externer Trigger, der die Verarbeitung startet. Beim Eintreten wird der Wert `NewValue` konvertiert und an das Zahlenfeld übertragen. |

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

| Name | Datentyp | Beschreibung |
|------|----------|--------------|
| `u16ObjId` | UINT | Objekt-ID des Ziel-Zahlenfeldes. Standardwert ist `ID_NULL`, kann aber über diesen Eingang überschrieben werden. |
| `NewValue` | SINT | Der anzuzeigende Wert, der vor der Übertragung nach UINT konvertiert wird. |

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Die Subapplikation arbeitet ereignisgesteuert. Ein externes Ereignis am Eingang `CNF` löst die Verarbeitung aus:

1. Das Ereignis wird an den Funktionsblock `F_SINT_TO_UINT` (aus der Bibliothek `iec61131::conversion`) weitergeleitet.
2. Gleichzeitig wird der aktuelle Wert von `NewValue` als Eingang `IN` an den Konvertierungsbaustein übergeben.
3. Nach Abschluss der Konvertierung signalisiert `F_SINT_TO_UINT` über seinen Ausgang `CNF` fertig.
4. Dieses Ereignis triggert den Baustein `Q_NumericValue` (aus `isobus::UT::Q`) mit dessen Eingang `REQ`.
5. Der konvertierte Wert (UINT) wird über den Ausgang `OUT` von `F_SINT_TO_UINT` an den Eingang `u32NewValue` von `Q_NumericValue` gelegt.
6. Die Objekt-ID `u16ObjId` wird direkt (ohne Änderung) an den entsprechenden Eingang `u16ObjId` von `Q_NumericValue` durchgereicht.

Somit wird der SINT-Wert in ein geeignetes Format umgewandelt und auf dem virtuellen Terminal angezeigt.

## Technische Besonderheiten

- Die Typkonvertierung nutzt den standardisierten FB `F_SINT_TO_UINT` aus der IEC‑61131‑Bibliothek, was eine robuste und normgerechte Umwandlung gewährleistet.
- Der Anzeigebaustein `Q_NumericValue` stammt aus der ISOBUS‑UT‑Bibliothek und erwartet einen 32‑Bit‑Wert; die Subapp übernimmt die notwendige Vorbereitung.
- Die Objekt-ID `u16ObjId` ist als `UINT` deklariert und mit dem Symbol `ID_NULL` vorbelegt, was eine einfache Parametrierung ermöglicht.
- Die Verbindung von `u16ObjId` zu `Q_NumericValue` ist als unsichtbar markiert, um die Übersichtlichkeit im Netzwerk zu erhöhen – sie wird aber dennoch vollständig ausgeführt.

## Zustandsübersicht

Die Subapp besitzt keinen expliziten Zustandsautomaten, da sie rein ereignisgesteuert arbeitet. Der Ablauf ist sequenziell:

- **Idle:** Warten auf ein Ereignis an `CNF`.
- **Konvertierung:** `NewValue` wird von SINT nach UINT konvertiert.
- **Ausgabe:** `Q_NumericValue` wird mit dem konvertierten Wert und der Objekt-ID angesteuert.

Nach Abschluss der Ausgabe kehrt die Subapp in den idle-Zustand zurück und kann erneut getriggert werden.

## Anwendungsszenarien

Typischer Einsatz ist die Visualisierung von numerischen Werten auf einem ISOBUS-Terminal (VT), z. B.:

- Anzeige der aktuellen Schrittkettennummer in einer Maschinensteuerung.
- Darstellung von SINT‑basierten Prozesswerten (Geschwindigkeit, Temperatur, etc.) auf einem Bediendisplay.
- Wiederverwendbare Komponente in größeren Applikationen, bei denen mehrere solche Anzeigen mit unterschiedlichen Objekt-IDs benötigt werden.

## Vergleich mit ähnlichen Bausteinen

Andere Bausteine zur Zahlenanzeige existieren häufig in direkter Form (z. B. `Q_NumericValue` selbst), erfordern aber bereits einen UINT-/DINT-Wert. Die Subapp `SINT_TO_Q_NumericValue` ergänzt die fehlende Typanpassung und kapselt die Konvertierung sowie die Ansteuerung des Zielbausteins. Gegenüber einer direkten Verwendung von `F_SINT_TO_UINT` und `Q_NumericValue` in einer Applikation bietet sie eine höhere Wiederverwendbarkeit und reduziert die Verdrahtung.

## Fazit

Die Subapp `SINT_TO_Q_NumericValue` ist eine kompakte, generische Lösung zur Anzeige von SINT-Werten auf einem ISOBUS‑VT. Sie kombiniert eine standardisierte Typkonvertierung mit dem etablierten Anzeigebaustein `Q_NumericValue` und ist durch die parametrierbare Objekt-ID flexibel einsetzbar. Die klare Schnittstelle und die einfache Logik machen sie zu einer sinnvollen Komponente in Automatisierungsapplikationen.