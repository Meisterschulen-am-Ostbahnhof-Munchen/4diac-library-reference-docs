# AR_MIN

## Einleitung

Der Funktionsbaustein `AR_MIN` dient der Ermittlung des Minimalwerts aus zwei analogen Eingangssignalen, die über Adapter (`AR`) übertragen werden. Er vergleicht die Datenwerte der beiden Eingangs-Sockets `IN0` und `IN1` und gibt den kleineren der beiden Werte an den Ausgangs-Plug `OUT` weiter.

Durch die konsequente Verwendung von Adaptern anstelle von diskreten Daten- und Ereignispins wird der Verdrahtungsaufwand im übergeordneten IEC 61499 Applikationsdiagramm signifikant reduziert.

## Schnittstellenstruktur

Da dieser Funktionsbaustein vollständig auf adapterbasierte Kommunikation setzt, verfügt er auf oberster Ebene über keine direkten, klassischen Event- oder Daten-Schnittstellen. Die gesamte Kommunikation wird über die deklarierten Adapter abgewickelt.

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden (Ereignisse werden über die Adapter-Schnittstellen empfangen).*

### **Ereignis-Ausgänge**

*Keine direkten Ereignis-Ausgänge vorhanden (Ereignisse werden über die Adapter-Schnittstellen gesendet).*

### **Daten-Eingänge**

*Keine direkten Daten-Eingänge vorhanden.*

### **Daten-Ausgänge**

*Keine direkten Daten-Ausgänge vorhanden.*

### **Adapter**

#### **Sockets (Eingangsschnittstellen)**

- **IN0** (Typ: `adapter::types::unidirectional::AR`):
  Der erste analoge Eingangssignal-Adapter.
- **IN1** (Typ: `adapter::types::unidirectional::AR`):
  Der zweite analoge Eingangssignal-Adapter.

#### **Plugs (Ausgangsschnittstellen)**

- **OUT** (Typ: `adapter::types::unidirectional::AR`):
  Der Ausgangsadapter. Er liefert den Minimalwert $\min(\text{IN0.D1}, \text{IN1.D1})$ inklusive des dazugehörigen Aktualisierungsereignisses.

---

## Funktionsweise

Sobald ein Ereignis `E1` an einem der beiden Eingangs-Adapter (`IN0` oder `IN1`) eintrifft, vergleicht der Baustein die aktuellen Datenwerte `IN0.D1` und `IN1.D1`:

$$\text{OUT.D1} = \min(\text{IN0.D1}, \text{IN1.D1})$$

Der ermittelte Minimalwert wird auf `OUT.D1` gelegt und zusammen mit dem Ausgangsereignis `OUT.E1` emittiert.

## Technische Besonderheiten

- **Unidirektionale Adapterstruktur**: Nutzt standardisierte `AR`-Adapterkanäle für eine saubere Signalrichtung.
- **Ereignisgesteuert**: Die Neuberechnung und Ausgabe erfolgt unmittelbar beim Eintreffen eines neuen Messwerts an `IN0` oder `IN1`.
- **Typkonformität**: Arbeitet intern auf Basis des Fließkommadatentyps `REAL`.

## Fazit

Der `AR_MIN` ist ein nützlicher Auswahlbaustein zur Bildung von Extremwerten (Mindestwertauswahl) in adapterbasierten IEC 61499 Anwendungen.
