# AX_AND_6

![AX_AND_6](AX_AND_6.svg)

* * * * * * * * * *

## Einleitung

Der `AX_AND_6` Funktionsblock ist ein generischer Baustein zur Berechnung einer logischen UND-Verknüpfung mit sechs unidirektionalen AX-Adapter-Eingängen. Er dient zur Bündelung und Sammelüberwachung von bis zu 6 Eingangssignalen (z. B. Not-Halt-Ketten, Freigabe- oder Statussignalen) zu einem gemeinsamen Adapter-Ausgangssignal.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**

Keine Ereignis-Ausgänge vorhanden.

### **Daten-Eingänge**

Keine direkten Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

Keine direkten Daten-Ausgänge vorhanden.

### **Adapter**

**Plug-Adapter (Ausgang):**

- **OUT** – UND-Ergebnis (Adaptertyp: `adapter::types::unidirectional::AX`)

**Socket-Adapter (Eingänge):**

- **IN1** – UND-Eingang 1 (Adaptertyp: `adapter::types::unidirectional::AX`)
- **IN2** – UND-Eingang 2 (Adaptertyp: `adapter::types::unidirectional::AX`)
- **IN3** – UND-Eingang 3 (Adaptertyp: `adapter::types::unidirectional::AX`)
- **IN4** – UND-Eingang 4 (Adaptertyp: `adapter::types::unidirectional::AX`)
- **IN5** – UND-Eingang 5 (Adaptertyp: `adapter::types::unidirectional::AX`)
- **IN6** – UND-Eingang 6 (Adaptertyp: `adapter::types::unidirectional::AX`)

## Funktionsweise

Der Funktionsblock wertet die logische UND-Funktion aller 6 AX-Eingänge aus. Der Ausgang `OUT.D1` ist nur dann `TRUE`, wenn alle sechs Sockets `IN1.D1` bis `IN6.D1` gleichzeitig `TRUE` sind. Sobald mindestens ein Eingang `FALSE` ist, schaltet der Ausgang auf `FALSE`.

## Änderungserkennung

Das Ergebnis wird nur auf den Ausgangs-Plug (`OUT`) geschrieben und dessen Adapter-Event `OUT.E1` nur gesendet, wenn sich der neu berechnete Wert vom aktuell gehaltenen Wert unterscheidet. Bleibt das logische Ergebnis unverändert, wird kein Adapter-Event gesendet – dies schützt nachgelagerte SubApp-Ketten vor überflüssigem Event-Spamming.

## Technische Besonderheiten

- **Generischer Funktionsblock**: Verwendet den System-Klassennamen `GEN_AX_AND` im Package `adapter::booleanOperators`.
- **Reine Adapter-Schnittstelle**: Ermöglicht eine direkte Anbindung ohne manuelle Daten-/Ereignissentkopplung.

## Anwendungsszenarien

- Sammelüberwachung von bis zu 6 Not-Halt- oder Schutzschalterkanälen (z. B. Not-Halt STG1 I1–I6 auf Diagnoseseiten).
- Bündelung mehrerer Sicherheits- und Freigabekriterien.

## Siehe auch

- [`AX_AND_2`](AX_AND_2.md) – 2-fach UND-Verknüpfung.
- [`AX_AND_3`](AX_AND_3.md) – 3-fach UND-Verknüpfung.
- [`AX_AND_4`](AX_AND_4.md) – 4-fach UND-Verknüpfung.
