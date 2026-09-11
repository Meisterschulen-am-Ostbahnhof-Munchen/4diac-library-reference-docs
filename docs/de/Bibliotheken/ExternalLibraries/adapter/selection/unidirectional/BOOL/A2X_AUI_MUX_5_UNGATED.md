# A2X_AUI_MUX_5_UNGATED

![A2X_AUI_MUX_5_UNGATED](./A2X_AUI_MUX_5_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **A2X_AUI_MUX_5_UNGATED** ist ein unidirektionaler 5:1-Multiplexer auf Basis von IEC-61499-Adaptern. Er wählt anhand des Indexes `K` einen von fünf A2X-Eingängen (`IN1` bis `IN5`) aus und gibt dessen Wert unverändert über den A2X-Ausgang `OUT` weiter.

Die Bezeichnung `UNGATED` kennzeichnet die besondere Eigenschaft dieses Bausteins: Er besitzt **keine Änderungserkennung**. Jedes neu berechnete Ergebnis des ausgewählten Eingangs wird bedingungslos weitergegeben, auch wenn der Wert gegenüber dem vorherigen Zyklus unverändert ist. Dadurch eignet er sich für Verbraucher, die eine periodische Kadenz benötigen, beispielsweise für Ableitungs- oder Frequenzberechnungen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

In der XML sind keine direkten Ereignis-Eingänge deklariert. Ereignisse zur Übertragung von Werten werden über die verwendeten Adaptertypen transportiert.

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `CNF` | Event | Bestätigung der Übernahme des eingestellten Indexes `K` |

### **Daten-Eingänge**

Es sind keine direkten Daten-Eingänge vorhanden. Die Nutzdaten werden über die A2X-Adapter `IN1` bis `IN5` sowie über den AUI-Adapter `K` eingelesen.

### **Daten-Ausgänge**

Es sind keine direkten Daten-Ausgänge vorhanden. Der Ausgangswert wird über den A2X-Adapter `OUT` bereitgestellt.

### **Adapter**

| Name | Richtung | Typ | Beschreibung |
|------|----------|-----|--------------|
| `OUT` | Plug / Ausgang | `adapter::types::unidirectional::A2X` | Ausgangswert; liefert den Wert des angewählten Eingangs |
| `K` | Socket / Eingang | `adapter::types::unidirectional::AUI` | Index zur Auswahl des aktiven Eingangs |
| `IN1` | Socket / Eingang | `adapter::types::unidirectional::A2X` | Eingangswert 1, ausgewählt bei `K = 0` |
| `IN2` | Socket / Eingang | `adapter::types::unidirectional::A2X` | Eingangswert 2, ausgewählt bei `K = 1` |
| `IN3` | Socket / Eingang | `adapter::types::unidirectional::A2X` | Eingangswert 3, ausgewählt bei `K = 2` |
| `IN4` | Socket / Eingang | `adapter::types::unidirectional::A2X` | Eingangswert 4, ausgewählt bei `K = 3` |
| `IN5` | Socket / Eingang | `adapter::types::unidirectional::A2X` | Eingangswert 5, ausgewählt bei `K = 4` |

## Funktionsweise

Der Baustein arbeitet als 5:1-Multiplexer:

1. Der Index `K` legt fest, welcher der fünf Eingänge aktiv ist:
   - `K = 0` → `IN1`
   - `K = 1` → `IN2`
   - `K = 2` → `IN3`
   - `K = 3` → `IN4`
   - `K = 4` → `IN5`

2. Der Wert des aktiven Eingangs wird über den Ausgang `OUT` weitergegeben.

3. Es findet keine Prüfung statt, ob sich der Eingangswert gegenüber dem vorherigen Zyklus verändert hat. Jedes neu berechnete Ergebnis wird unmittelbar weitergeleitet.

4. Der Ereignisausgang `CNF` bestätigt die Übernahme des eingestellten Indexes `K`.

Durch das Fehlen einer Änderungserkennung bleibt die Ausgangskadenz vollständig von der Berechnungskadenz der Eingangsquelle abhängig. Auch unveränderte Werte erzeugen eine Ausgabe.

## Technische Besonderheiten

- Der Baustein ist als generischer Funktionsblock deklariert. Das Attribut `eclipse4diac::core::GenericClassName` verweist auf die generische Implementierung `GEN_A2X_AUI_MUX`.
- Die Schnittstelle verwendet ausschließlich unidirektionale Adapter des Typs `adapter::types::unidirectional`.
- Es gibt keine direkten IEC-61499-Daten-Ein- oder -Ausgänge; der Datentransport erfolgt vollständig über Adapter.
- Der Baustein ist im Compiler-Paket `adapter::selection::unidirectional` abgelegt.
- Der `TypeHash` ist in der XML noch leer.
- Laut Versionshinweis ist das zugehörige Adapter-Backend `GEN_A2X_AUI_MUX` noch zu implementieren.

## Zustandsübersicht

Die XML enthält keinen expliziten Ausführungszustandsautomaten (ECC). Der Baustein verhält sich als zustandsarmer Multiplexer:

| Phase | Beschreibung |
|-------|--------------|
| Initial | Warten auf einen gültigen Index `K` und anliegende Eingangswerte |
| Auswahl | Der zu `K` gehörende Eingang wird an den Ausgang `OUT` gekoppelt |
| Weiterleitung | Jeder neu berechnete Wert des aktiven Eingangs wird unmittelbar an `OUT` weitergegeben |
| Umschaltung | Bei Änderung von `K` wird die Auswahl aktualisiert; `CNF` bestätigt den neuen Index |

## Anwendungsszenarien

- **Periodische Frequenz- oder Ableitungsberechnung:** Der nachgelagerte Verbraucher benötigt einen kontinuierlichen Datenstrom, auch wenn sich der Wert nicht ändert.
- **Sensormultiplexing:** Fünf Messwerte werden über A2X-Adapter eingelesen und je nach Index gemeinsam auf einen Ausgang geschaltet.
- **Adapterbasierte Signalverarbeitung:** Der Baustein lässt sich in IEC-61499-Systeme integrieren, die durchgängig mit unidirektionalen Adaptern arbeiten.
- **Umschaltung zwischen mehreren Berechnungsquellen:** Eine nachgeschaltete Funktion soll immer das aktuellste Ergebnis der ausgewählten Quelle erhalten.

## Vergleich mit ähnlichen Bausteinen

- **A2X_AUI_MUX_5:** Besitzt eine Änderungserkennung und gibt nur dann einen Wert weiter, wenn sich das Ergebnis tatsächlich geändert hat. Der `UNGATED`-Baustein verzichtet auf diese Erkennung.
- **AX_AUI_MUX_5_UNGATED:** Bietet dieselbe Multiplex-Logik, verwendet jedoch einen anderen Adaptertyp (`AX` statt `A2X`). `A2X_AUI_MUX_5_UNGATED` ist die A2X-Variante davon.
- **Klassischer MUX mit Daten-Eingängen:** Ein Standard-Multiplexer besitzt sichtbare Daten-Eingänge und einen skalareren Index-Eingang. Der vorliegende Baustein kapselt die Signale in Adaptertypen und ist dadurch für adapterbasierte Verbindungen besser geeignet.

## Fazit

**A2X_AUI_MUX_5_UNGATED** ist ein flexibler 5:1-Multiplexer für adapterbasierte 4diac-Systeme. Seine besondere Stärke ist die bedingungslose Weitergabe jedes neu berechneten Ergebnisses. Dadurch ist er ideal für Anwendungen, die eine lückenlose, periodische Datenversorgung benötigen und keine unveränderten Werte unterdrücken dürfen.
