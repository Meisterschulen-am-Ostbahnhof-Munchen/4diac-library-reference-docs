# A2X_AUI_MUX_3_UNGATED

![A2X_AUI_MUX_3_UNGATED](./A2X_AUI_MUX_3_UNGATED.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein **A2X_AUI_MUX_3_UNGATED** ist ein generischer Multiplexer auf Adapterbasis. Er wählt aus drei A2X-Eingangsadaptern einen Datenstrom aus und leitet ihn an einen A2X-Ausgangsadapter weiter. Die Auswahl erfolgt über den AUI-Adapter `K`.

Im Gegensatz zur Variante mit Änderungserkennung besitzt dieser Baustein keine „Gating“-Logik: Jedes neu berechnete Ergebnis wird bedingungslos weitergegeben, unabhängig davon, ob sich der Wert geändert hat. Dadurch eignet er sich besonders für Verbraucher, die eine periodische Kadenz benötigen, etwa für Ableitungs- oder Frequenzberechnungen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
| --- | --- | --- |
| `CNF` | Event | Bestätigung des gesetzten Index `K` |

### **Daten-Eingänge**

Keine. Die Datenübertragung erfolgt vollständig über Adapter.

### **Daten-Ausgänge**

Keine. Die Datenübertragung erfolgt vollständig über Adapter.

### **Adapter**

| Name | Typ | Richtung | Kommentar |
| --- | --- | --- | --- |
| `OUT` | `adapter::types::unidirectional::A2X` | Plug | Ausgangsadapter; liefert `IN1` bei `K = 0`, `IN2` bei `K = 1`, `IN3` bei `K = 2` |
| `K` | `adapter::types::unidirectional::AUI` | Socket | Indexadapter zur Auswahl des aktiven Eingangs |
| `IN1` | `adapter::types::unidirectional::A2X` | Socket | Eingangswert 1, ausgewählt bei `K = 0` |
| `IN2` | `adapter::types::unidirectional::A2X` | Socket | Eingangswert 2, ausgewählt bei `K = 1` |
| `IN3` | `adapter::types::unidirectional::A2X` | Socket | Eingangswert 3, ausgewählt bei `K = 2` |

## Funktionsweise

Der Baustein arbeitet als Adapter-Multiplexer:

1. Über den AUI-Adapter `K` wird ein Index empfangen.
2. Abhängig vom Index wird einer der drei A2X-Eingänge ausgewählt:
   - `K = 0` → `IN1`
   - `K = 1` → `IN2`
   - `K = 2` → `IN3`
3. Die Daten des ausgewählten Eingangs werden auf den Ausgangsadapter `OUT` durchgeschaltet.
4. Über das Ereignis `CNF` wird bestätigt, dass der Index `K` übernommen wurde.

Jedes an einem aktiven Eingang ankommende Ergebnis wird weitergeleitet. Eine Prüfung, ob sich der Wert gegenüber der vorherigen Übertragung geändert hat, findet nicht statt.

## Technische Besonderheiten

- **Keine Änderungserkennung:** Die Bezeichnung `UNGATED` bedeutet, dass keine Wertänderungserkennung erfolgt.
- **Reine Adapter-Schnittstelle:** Es gibt weder Daten-Ein- noch Daten-Ausgänge; der Datentransport erfolgt ausschließlich über unidirektionale Adapter.
- **Generischer Funktionsbaustein:** Der Baustein ist als generischer FB deklariert und verwendet die generische Klasse `GEN_A2X_AUI_MUX`.
- **Kein Ereignis-Eingang:** Die Steuerung erfolgt nicht über klassische Event-Eingänge, sondern über die Adapterverbindungen.
- **Periodische Weitergabe:** Durch das Fehlen einer Änderungserkennung eignet sich der Baustein für Anwendungen, die eine kontinuierliche, periodische Datenweitergabe benötigen.

## Zustandsübersicht

Der Baustein besitzt keine explizit ausgeprägte Zustandsmaschine. Der Auswahlzustand wird durch den Wert von `K` bestimmt:

| `K` | ausgewählter Eingang |
| --- | --- |
| `0` | `IN1` |
| `1` | `IN2` |
| `2` | `IN3` |

Gültige Indexwerte sind `0`, `1` und `2`. Werte außerhalb dieses Bereichs sind in der Schnittstellenbeschreibung nicht definiert.

## Anwendungsszenarien

- **Sensorumschaltung:** Auswahl eines von drei analogen Messwerten für einen gemeinsamen Verarbeitungspfad.
- **Zyklische Datenweitergabe:** Weitergabe jedes Berechnungsergebnisses mit fester Kadenz, auch wenn sich der Wert nicht ändert.
- **Ableitungs- und Frequenzberechnung:** Einsatz vor Bausteinen, die aus zeitlichen Wertfolgen Differenzen oder Frequenzen berechnen.
- **Adapterbasiertes Multiplexing:** Strukturierte Kopplung mehrerer A2X-Datenquellen an einen einzelnen Ausgangsadapter.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Änderungserkennung | Datenadapter | Besonderheit |
| --- | --- | --- | --- |
| `A2X_AUI_MUX_3` | Ja | A2X | Gibt Werte nur bei Änderung weiter. |
| `A2X_AUI_MUX_3_UNGATED` | Nein | A2X | Gibt jedes neu berechnete Ergebnis bedingungslos weiter. |
| `AX_AUI_MUX_3_UNGATED` | Nein | AX | Variante mit anderem Datenadaptertyp. |

## Fazit

`A2X_AUI_MUX_3_UNGATED` ist ein flexibler, adapterbasierter Multiplexer für drei A2X-Datenströme. Durch den Verzicht auf eine Änderungserkennung liefert er jedes neue Ergebnis sofort weiter. Damit ist er ideal für Anwendungen, die eine kontinuierliche, periodische Datenverarbeitung erfordern und keine Stillstandszeiten durch unveränderte Werte erlauben.
