# AR_CALIBRATE_SQ_REF


![AR_CALIBRATE_SQ_REF_ecc](./AR_CALIBRATE_SQ_REF_ecc.svg)

![AR_CALIBRATE_SQ_REF](./AR_CALIBRATE_SQ_REF.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AR_CALIBRATE_SQ_REF** realisiert eine zweipunktige Sequenzkalibrierung (Offset, dann Scale) für einen linearen Messkanal. Er basiert auf Adaptern und erzwingt über die ECC (Execution Control Chart) eine strikte Reihenfolge: Zuerst muss der Offset kalibriert werden (`CO`), danach die Skalierung (`CS`). Die Zielwerte für die Kalibrierung (`Y_Offset`, `Y_Scale`) werden nicht als einfache Daten-Eingänge, sondern als bidirektionale AR2-Sockets live von einer Visualisierung (VT) oder einem Web-Interface bereitgestellt und über zusätzliche AR2-Plugs (`ZERO`, `SPAN`) persistent gespeichert. Das Modul berechnet den kalibrierten Ausgabewert nach der Formel `Y = (X + OFFSET) * SCALE`.

## Schnittstellenstruktur

Der Baustein besitzt keine direkten Ereignis- oder Datenports. Der gesamte Datenaustausch erfolgt über Adapter-Schnittstellen. Die folgenden Abschnitte fassen die verwendeten Signale zusammen.

### **Ereignis-Eingänge**

Es existieren keine direkten Ereignis-Eingänge. Ereignisse werden ausschließlich über die angeschlossenen Adapter empfangen:

- **X.E1** – Ereignis vom Rohwert-Socket (unidirektionaler Adapter `AR`)
- **CO.E1** – Ereignis zur Anforderung der Offset-Kalibrierung (Socket `AX`)
- **CS.E1** – Ereignis zur Anforderung der Scale-Kalibrierung (Socket `AX`)
- **OFFSET.EI1** – Bestätigung des persistierten Offset-Werts (Plug `AR2`)
- **SCALE.EI1** – Bestätigung des persistierten Scale-Werts (Plug `AR2`)
- **ZERO.EI1** – Bestätigung des persistierten Nullpunkt-Referenzwerts (Plug `AR2`)
- **SPAN.EI1** – Bestätigung des persistierten Spanne-Referenzwerts (Plug `AR2`)
- **Y_Offset.EO1** – Live-Änderung des Offset-Referenzwerts von einem externen Plug (Socket `AR2`)
- **Y_Scale.EO1** – Live-Änderung des Scale-Referenzwerts von einem externen Plug (Socket `AR2`)

### **Ereignis-Ausgänge**

Es existieren keine direkten Ereignis-Ausgänge. Ereignisse werden über Adapter gesendet:

- **Y.E1** – Ereignis, dass der kalibrierte Ausgabewert gültig ist (Plug `AR`)
- **OFFSET.EO1** – Ereignis zum Schreiben des neuen Offset-Werts in den Persistenzspeicher (Plug `AR2`)
- **SCALE.EO1** – Ereignis zum Schreiben des neuen Scale-Werts in den Persistenzspeicher (Plug `AR2`)
- **ZERO.EO1** – Ereignis zum Schreiben des Nullpunkt-Referenzwerts in den Persistenzspeicher (Plug `AR2`)
- **SPAN.EO1** – Ereignis zum Schreiben des Spanne-Referenzwerts in den Persistenzspeicher (Plug `AR2`)
- **Y_Offset.EI1** – Ereignis zum Rücksenden des Offset-Referenzwerts für die Anzeige (Socket `AR2`)
- **Y_Scale.EI1** – Ereignis zum Rücksenden des Scale-Referenzwerts für die Anzeige (Socket `AR2`)

### **Daten-Eingänge**

Es gibt keine direkten Daten-Eingänge. Folgende Adapter-Daten werden empfangen:

- **X.D1** – Rohwert (REAL) vom Sensor (Socket `AR`)
- **OFFSET.DI1** – persistierter Offset-Wert (REAL) vom Persistenzspeicher (Plug `AR2`)
- **SCALE.DI1** – persistierter Scale-Wert (REAL) vom Persistenzspeicher (Plug `AR2`)
- **ZERO.DI1** – persistierter Nullpunkt-Referenzwert (REAL) vom Persistenzspeicher (Plug `AR2`)
- **SPAN.DI1** – persistierter Spanne-Referenzwert (REAL) vom Persistenzspeicher (Plug `AR2`)
- **Y_Offset.DO1** – neuer Offset-Referenzwert (REAL) vom externen Plug (Socket `AR2`)
- **Y_Scale.DO1** – neuer Scale-Referenzwert (REAL) vom externen Plug (Socket `AR2`)

### **Daten-Ausgänge**

Es gibt keine direkten Daten-Ausgänge. Folgende Adapter-Daten werden gesendet:

- **Y.D1** – kalibrierter Ausgabewert (REAL) (Plug `AR`)
- **OFFSET.DO1** – neuer Offset-Wert, an den Persistenzspeicher zu senden (Plug `AR2`)
- **SCALE.DO1** – neuer Scale-Wert, an den Persistenzspeicher zu senden (Plug `AR2`)
- **ZERO.DO1** – Nullpunkt-Referenzwert, an den Persistenzspeicher zu senden (Plug `AR2`)
- **SPAN.DO1** – Spanne-Referenzwert, an den Persistenzspeicher zu senden (Plug `AR2`)
- **Y_Offset.DI1** – rückgesendeter Offset-Referenzwert für Anzeige und Web (Socket `AR2`)
- **Y_Scale.DI1** – rückgesendeter Scale-Referenzwert für Anzeige und Web (Socket `AR2`)

### **Adapter**

Der Baustein verwendet folgende Adapter-Typen:

| Adapter | Typ | Richtung | Beschreibung |
|---------|-----|----------|--------------|
| `X` | `unidirectional::AR` | Socket | Rohwert-Eingang (Ereignis + Daten) |
| `Y` | `unidirectional::AR` | Plug | Kalibrierter Ausgang (Ereignis + Daten) |
| `CO` | `unidirectional::AX` | Socket | Auslöser für Offset-Kalibrierung |
| `CS` | `unidirectional::AX` | Socket | Auslöser für Scale-Kalibrierung |
| `OFFSET` | `bidirectional::AR2` | Plug | Persistierter Offset-Wert (Lesen/Schreiben) |
| `SCALE` | `bidirectional::AR2` | Plug | Persistierter Scale-Wert (Lesen/Schreiben) |
| `ZERO` | `bidirectional::AR2` | Plug | Persistierter Nullpunkt-Referenzwert (Lesen/Schreiben) |
| `SPAN` | `bidirectional::AR2` | Plug | Persistierter Spanne-Referenzwert (Lesen/Schreiben) |
| `Y_Offset` | `bidirectional::AR2` | Socket | Live-Referenzwert für Offset (Daten von extern, Echo an Display) |
| `Y_Scale` | `bidirectional::AR2` | Socket | Live-Referenzwert für Scale (Daten von extern, Echo an Display) |

## Funktionsweise

Der Funktionsblock arbeitet in zwei Kalibrierphasen:

1. **Offset-Kalibrierung (`CO`)**  
   Der Anwender legt eine bekannte Referenz am Eingang `X` an und setzt den gewünschten Ausgabewert `Y_Offset` auf die Zielgröße. Durch Auslösen von `CO` berechnet der Baustein den Offset so, dass nach dem Algorithmus `Y = (X + OFFSET) * SCALE` garantiert `Y = ZERO` gilt. Die Berechnung verwendet den aktuellen Scale-Wert (falls vorhanden) und die gespeicherten Referenzdaten.

2. **Scale-Kalibrierung (`CS`)**  
   Nach der Offset-Kalibrierung wird eine zweite Referenz angelegt und `Y_Scale` als gewünschter Ausgabewert gesetzt. Durch `CS` werden Scale und Offset neu berechnet, sodass die Kennlinie exakt durch beide Referenzpunkte verläuft. Der Scale-Wert wird nur akzeptiert, wenn `X - X_LOW_INT` ungleich Null ist (Division durch Null vermieden).

Nach jeder Kalibrierung werden die neuen Werte über die AR2-Plugs (`OFFSET`, `SCALE`) an einen externen Persistenzspeicher (z. B. `INI_AR2`) gesendet. Nach Bestätigung durch den Speicher (Ereignisse `OFFSET.EI1` bzw. `SCALE.EI1`) sind sie für die weitere Berechnung gültig.

Die Live-Referenzwerte `Y_Offset` und `Y_Scale` werden über bidirektionale Sockets eingelesen. Sobald ein neuer Wert über `Y_Offset.EO1` bzw. `Y_Scale.EO1` ankommt, wird er sofort an die Plug-Adapter `ZERO` bzw. `SPAN` weitergegeben und dort persistiert. Bei jedem Persistenzzyklus wird der gespeicherte Wert über `ZERO.EI1` bzw. `SPAN.EI1` zurückgelesen und anschließend über `Y_Offset.EI1` bzw. `Y_Scale.EI1` an die anzeigende Einheit zurückgesendet – so erscheinen auch beim Boot oder nach einer Änderung die aktuell gespeicherten Werte auf der Visualisierung.

## Technische Besonderheiten

- **Adapterbasierte Kommunikation**: Alle Ein- und Ausgänge sind als Adapter realisiert. Es gibt keine klassischen Daten- oder Event-Ports.
- **Bidirektionale AR2-Adapter**: Für die Persistierung (`OFFSET`, `SCALE`, `ZERO`, `SPAN`) sowie für die Live-Referenzen (`Y_Offset`, `Y_Scale`) werden bidirektionale AR2-Adapter verwendet. Bei Sockets ist die Ein-/Ausgangsrichtung vertauscht: `EO1/DO1` sind Eingänge (vom externen Plug), `EI1/DI1` Ausgänge (an den externen Plug). Bei Plugs gilt die übliche Richtung.
- **ECC-erzwungene Reihenfolge**: Der Zustand `WAIT_CS` verhindert, dass eine Skalierung ohne vorherigen Offset-Kalibrierungsschritt ausgeführt wird. Die Übergänge sind ausschließlich durch die Zustandsmaschine definiert.
- **Round-Trip-Persistierung**: Kalibrierte Werte werden nie direkt aus den Live-Sockets für die Mathematik gelesen, sondern immer aus den rückbestätigten Persistenzadaptern (`OFFSET.DI1`, `SCALE.DI1`, `ZERO.DI1`, `SPAN.DI1`). Dadurch wird sichergestellt, dass nur tatsächlich gespeicherte Werte verwendet werden.
- **Boot/Display-Echo**: Beim Start oder nach einer Änderung werden die persistierten Referenzwerte automatisch an die Visualisierung zurückgespiegelt, sodass der Bediener den aktuellen Zustand ohne manuelle Eingabe sieht.
- **Division durch Null**: Im `CS`-Algorithmus wird eine Division durch Null vermieden, und der Scale-Wert wird nur aktualisiert, wenn die Differenz der X-Werte ungleich Null ist.

## Zustandsübersicht

Die Zustandsmaschine des FB umfasst folgende Zustände:

| Zustand | Bedeutung |
|---------|-----------|
| `IDLE` | Warten auf Ereignisse (Standardzustand) |
| `REQ` | Berechnung des kalibrierten Ausgangs (`Y = (X + OFFSET) * SCALE`) und Senden über `Y.E1` |
| `CO` | Offset-Kalibrierung: aktuelle `X`- und `ZERO.DI1`-Werte werden intern gespeichert, neuer Offset wird berechnet und über `OFFSET.EO1` ausgegeben |
| `WAIT_CS` | Wartet, dass entweder eine Scale-Kalibrierung (`CS.E1`) oder eine erneute Offset-Kalibrierung (`CO.E1`) ausgelöst wird |
| `REQ_WAIT` | Zwischenzustand, in dem eine Berechnung während des Wartens auf Scale durchgeführt wird |
| `CS` | Scale-Kalibrierung: Berechnung von Scale und Offset unter Verwendung der gespeicherten Werte aus der Offset-Phase |
| `ZERO_ECHO` | Persistiert den neu eingetroffenen `Y_Offset.DO1` direkt nach `ZERO` (Plug) |
| `SPAN_ECHO` | Persistiert den neu eingetroffenen `Y_Scale.DO1` direkt nach `SPAN` (Plug) |
| `Y_OFFSET_DISP` | Echo des persistierten `ZERO.DI1`-Werts zurück an die Anzeige über `Y_Offset.EI1` |
| `Y_SCALE_DISP` | Echo des persistierten `SPAN.DI1`-Werts zurück an die Anzeige über `Y_Scale.EI1` |

Der ECC erzwingt: Nach `CO` ist nur der Übergang nach `WAIT_CS` möglich; von dort kann entweder `CS` oder erneut `CO` (Wiederholung der Offset-Kalibrierung) erreicht werden. Die Berechnung (`X.E1`) ist in allen Zuständen möglich.

## Anwendungsszenarien

- **Messtechnik / Sensorik**: Kalibrierung von Messumformern, die eine lineare Kennlinie mit Offset und Verstärkung aufweisen.
- **Prozessautomatisierung**: Anpassung analoger Messwerte an Sollwerte in Steuerungen.
- **Visualisierungsgestützte Kalibrierung**: Besonders geeignet für Bedienoberflächen, da die Zielwerte live über Web/Visualisierung geändert und die persistierten Werte direkt zurückgespiegelt werden.
- **Qualitätssicherung**: Zweipunktkalibrierung mit garantierter Reihenfolge und gespeicherten Referenzen zur Rückverfolgbarkeit.

## Vergleich mit ähnlichen Bausteinen

Der Funktionsblock `AR_CALIBRATE_SQ_REF` unterscheidet sich von einem einfacheren Baustein wie `AR_CALIBRATE_SQ` dadurch, dass die Kalibrierziele (`Y_Offset`, `Y_Scale`) nicht als lokale Eingangsvariablen über einen `SET`-Ereignis gesetzt werden, sondern als **live bidirektionale AR2-Sockets** von externen Quellen (VT/Web) gespeist werden. Dadurch wird eine unmittelbare, externe Steuerung der Referenzwerte ermöglicht, ohne Änderungen an der Konfiguration des Bausteins selbst. Zusätzlich wird die Persistierung über zwei separate AR2-Plugs (`ZERO`, `SPAN`) erzwungen, während andere Bausteine oft nur interne Variablen verwenden.

## Fazit

`AR_CALIBRATE_SQ_REF` bietet eine robuste, adapterbasierte Zweipunktkalibrierung mit erzwungener Reihenfolge und vollständiger Persistierung der Kalibrierparameter. Die Verwendung von bidirektionalen AR2-Adaptern ermöglicht eine nahtlose Integration in Visualisierungs- und Persistenzsysteme und stellt sicher, dass alle Werte konsistent bleiben – auch nach einem Neustart. Die klare Trennung zwischen Live-Referenzen und gespeicherten Werten sowie das Echo an die Anzeige machen den Baustein besonders für anspruchsvolle Automatisierungslösungen geeignet, bei denen eine hohe Genauigkeit und Wiederholbarkeit gefordert sind.
