# AE_MERGE_2

![AE_MERGE_2](./AE_MERGE_2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AE_MERGE_2** führt zwei unidirektionale AE-Adapter (reines Ereignis, keine Nutzdaten) auf einen gemeinsamen Ausgangsadapter zusammen. Er ist die Umkehrung von [AE_SPLIT_2](AE_SPLIT_2.md): Statt ein Ereignis auf zwei Ausgänge zu verteilen, führt er zwei unabhängige Ereignisquellen auf einen gemeinsamen Ausgang. Der Baustein ist als generischer FB implementiert (`GenericClassName: 'GEN_AE_MERGE'`).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine. Die Ereignisübernahme erfolgt ausschließlich über die Adapter-Sockets.

### **Ereignis-Ausgänge**

Keine. Die Ereignisweitergabe erfolgt ausschließlich über den Adapter-Plug.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| ------- | ------ | ----- | -------------- |
| Socket (Eingang 1) | `IN1` | `adapter::types::unidirectional::AE` | Erste Ereignisquelle. |
| Socket (Eingang 2) | `IN2` | `adapter::types::unidirectional::AE` | Zweite Ereignisquelle. |
| Plug (Ausgang) | `OUT` | `adapter::types::unidirectional::AE` | Zusammengeführtes Ereignis. |

## Funktionsweise

Trifft ein Ereignis an `IN1` **oder** `IN2` ein, wird es unverändert an `OUT` weitergereicht. Da `AE` keinen Datenwert transportiert, gibt es beim Zusammenführen nichts zu arbitrieren außer dem Ereignis selbst – jedes einzelne eingehende Ereignis erscheint unmittelbar am Ausgang, unabhängig davon, von welchem Socket es kam. Die Reihenfolge, in der beide Sockets auf ein gleichzeitiges Eintreffen geprüft werden, ist implementierungsseitig `IN1` vor `IN2`, was bei zwei innerhalb desselben Ausführungszyklus eintreffenden Ereignissen relevant werden kann.

## Technische Besonderheiten

- **Generischer Typ**: Implementiert über die generische Basisklasse `CGenUnidirectMergeBase` (N Sockets, 1 Plug) – dieselbe C++-Basis, die auch `GEN_ASR_MERGE` und `GEN_ASRT_MERGE` zugrunde liegt. Die Anzahl der Eingangs-Sockets wird über den generischen Suffix (`_2`) zur Übersetzungszeit festgelegt.
- **Keine Zustände / Algorithmen**: Da `AE` keinen Datenwert trägt, gibt es keine Änderungserkennung und kein ECC – der Baustein ist reine Ereignisweiterleitung.
- **Kein Datenverlust bei gleichzeitigen Ereignissen**: Anders als eine reine Datenverbindung (die nur eine Quelle erlaubt) kann jedes Ereignis von `IN1` und `IN2` unabhängig durchgereicht werden; es geht kein Ereignis „verloren“, nur weil das jeweils andere Socket kurz zuvor gefeuert hat.

## Zustandsübersicht

Der Baustein besitzt **keinen Zustandsautomaten**. Das Verhalten ist rein kombinatorisch: Jedes eingehende Ereignis an `IN1` oder `IN2` wird unmittelbar an `OUT` weitergereicht.

## Anwendungsszenarien

- **Ereigniszusammenführung**: Zwei unabhängige Ereignisquellen (z. B. zwei Taster oder zwei Sensor-Trigger) sollen dieselbe Folgeaktion auslösen, ohne dass die nachgeschaltete Logik zweimal verdrahtet werden muss.
- **Redundante Auslösepfade**: Ein Ereignis kann über zwei unabhängige Quellen ausgelöst werden (z. B. ein manueller und ein automatischer Pfad), die beide auf denselben Ausgang münden.
- **Vereinfachung von Netzwerken**, die sonst zwei separate Ereignisverbindungen zum selben Ziel benötigen würden.

## Vergleich mit ähnlichen Bausteinen

- **[AE_SPLIT_2](AE_SPLIT_2.md)**: die Umkehrrichtung – verteilt ein eingehendes Ereignis auf zwei Ausgänge, statt zwei Eingänge zusammenzuführen.
- **[ASR_MERGE_2](ASR_MERGE_2.md)**: dieselbe Zusammenführungslogik für den Set/Reset-Ereignisadapter `ASR` (2 Ereignisse statt 1).
- **[ASRT_MERGE_2](ASRT_MERGE_2.md)**: dieselbe Zusammenführungslogik für den Set/Reset/Toggle-Ereignisadapter `ASRT` (3 Ereignisse statt 1).

## Fazit

Der **AE_MERGE_2** ist ein minimaler generischer Baustein zur Zusammenführung zweier reiner Ereignisquellen auf einen gemeinsamen AE-Adapterausgang. Seine fehlende Logik sorgt für minimale Latenz, während die generische Implementierung ihn konsistent mit den übrigen `GEN_*_MERGE`-Bausteinen der Familie macht.
