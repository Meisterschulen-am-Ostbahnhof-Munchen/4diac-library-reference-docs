# AUDI_AUI_MUX_6_VAL


![AUDI_AUI_MUX_6_VAL_network](./AUDI_AUI_MUX_6_VAL_network.svg)

![AUDI_AUI_MUX_6_VAL](./AUDI_AUI_MUX_6_VAL.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AUDI_AUI_MUX_6_VAL** ist ein 6-Wege-Multiplexer für AUDI‑Adapterwerte. Er wählt über Ereignis-Eingänge einen von sechs UDINT‑Datenwerten aus und stellt diesen als AUDI‑Adapter am Ausgang bereit. Intern kombiniert er Ereignis‑ und Adapter‑Multiplexer sowie Initialisierungseinheiten, um die eingehenden Werte zunächst in AUDI‑Adapter zu konvertieren und anschließend den aktiven Kanal durchzuschalten. Die Auswahl erfolgt ausschließlich über die Ereignis-Eingänge **EI1** bis **EI6**, wodurch eine zeitlich präzise Umschaltung ermöglicht wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ    | Kommentar                                      |
|------|--------|------------------------------------------------|
| EI1  | Event  | Event zur Auswahl von **val1**                |
| EI2  | Event  | Event zur Auswahl von **val2**                |
| EI3  | Event  | Event zur Auswahl von **val3**                |
| EI4  | Event  | Event zur Auswahl von **val4**                |
| EI5  | Event  | Event zur Auswahl von **val5**                |
| EI6  | Event  | Event zur Auswahl von **val6**                |

### **Ereignis-Ausgänge**

Keine Ereignis-Ausgänge vorhanden.

### **Daten-Eingänge**

| Name | Typ     | Kommentar                                      |
|------|---------|------------------------------------------------|
| val1 | UDINT   | Initialer Ausgabewert bei EI1                  |
| val2 | UDINT   | Initialer Ausgabewert bei EI2                  |
| val3 | UDINT   | Initialer Ausgabewert bei EI3                  |
| val4 | UDINT   | Initialer Ausgabewert bei EI4                  |
| val5 | UDINT   | Initialer Ausgabewert bei EI5                  |
| val6 | UDINT   | Initialer Ausgabewert bei EI6                  |

### **Daten-Ausgänge**

Keine direkten Daten-Ausgänge vorhanden; die Ausgabe erfolgt ausschließlich über den Adapter.

### **Adapter**

| Name | Typ                                                                                   | Kommentar                          |
|------|---------------------------------------------------------------------------------------|------------------------------------|
| OUT  | `adapter::types::unidirectional::AUDI`                                               | Ausgewählter AUDI‑Adapter-Output  |

## Funktionsweise

Der Funktionsblock ist intern als Subapplikation realisiert und besteht aus mehreren Komponenten:

1. **Werteinitialisierung**: Die sechs externen UDINT‑Werte **val1** … **val6** werden jeweils einer Instanz des Bausteins `initval_AUDI` zugeführt. Diese Instanzen wandeln den übergebenen Zahlenwert in einen AUDI‑Adapter um und stellen ihn an ihrem `OUT`‑Port bereit.

2. **Ereignis-Multiplexer**: Der Baustein `AUI_MUX_6` empfängt die externen Ereignisse **EI1** … **EI6**. Anhand dieser Ereignisse erzeugt er ein Auswahlsignal (am Port `K`), das den gewünschten Kanal für den nachgeschalteten Adapter-Multiplexer festlegt.

3. **Adapter-Multiplexer**: Der Baustein `AUDI_AUI_MUX_6` verfügt über sechs Eingänge (`IN1` … `IN6`), an denen die initialisierten AUDI‑Adapter anliegen, sowie über einen Steuereingang `K`. Der Steuerwert bestimmt, welcher der sechs Eingänge auf den Ausgang `OUT` durchgeschaltet wird.

4. **Ausgabe**: Der ausgewählte AUDI‑Adapter wird über den externen Adapter‑Port `OUT` ausgegeben.

Durch die Kombination dieser Bausteine realisiert `AUDI_AUI_MUX_6_VAL` eine zuverlässige und ereignisgesteuerte Umschaltung zwischen sechs vordefinierten Werten, ohne dass zusätzliche Logik von außen implementiert werden muss.

## Technische Besonderheiten

- **Struktur als Subapplikation**: Der FB ist als SubApp definiert und nutzt dabei bereits vorhandene Typen (`AUI_MUX_6`, `AUDI_AUI_MUX_6` und `initval_AUDI`). Dies erlaubt eine kompakte Wiederverwendung erprobter Bausteine und eine klare Modularisierung.
- **Datenkonvertierung**: Eingangsdaten sind vom Typ `UDINT`. Durch die `initval_AUDI`‑Instanzen werden diese Werte in den spezifischen Adaptertyp `AUDI` transformiert, der für die Ausgabe benötigt wird.
- **Exklusive Ereignissteuerung**: Die Auswahl des aktiven Kanals erfolgt ausschließlich über Ereignis-Eingänge. Es gibt keinen Daten‑Eingang für die Auswahl, was die Kopplung zwischen Ereignis und Wertwechsel sehr direkt gestaltet.
- **Keine Rückmeldung über Ereignis‑Ausgänge**: Nach erfolgreicher Umschaltung wird kein spezielles Ereignis erzeugt; der Zustand ist implizit über das zuletzt anstehende Ereignis definiert.

## Zustandsübersicht

Da der Baustein ereignisgesteuert arbeitet, gibt es keine persistenten internen Zustände im eigentlichen Sinne. Nach jedem Ereignis wird der entsprechende Kanal gesetzt und bleibt aktiv, bis ein weiteres Ereignis eintrifft:

| Zustand  | Auslöser | Ergebnis (Ausgang `OUT`) |
|----------|----------|--------------------------|
| Kanal 1  | EI1      | Adapter aus `val1`       |
| Kanal 2  | EI2      | Adapter aus `val2`       |
| Kanal 3  | EI3      | Adapter aus `val3`       |
| Kanal 4  | EI4      | Adapter aus `val4`       |
| Kanal 5  | EI5      | Adapter aus `val5`       |
| Kanal 6  | EI6      | Adapter aus `val6`       |

Überlappende Ereignisse sind nicht definiert; es wird allgemein erwartet, dass jeweils nur ein Ereignis aktiv ist.

## Anwendungsszenarien

- **Konfigurationsumschaltung** in Automatisierungssystemen, bei denen je nach Betriebsmodus unterschiedliche Parameterwerte (z. B. Geschwindigkeitsgrenzen, Temperatursollwerte) als AUDI‑Daten an eine übergeordnete Steuerung übergeben werden.
- **Quellenselektion** in Multimedia‑ oder Datenübertragungssystemen, bei denen zwischen mehreren Signalquellen umgeschaltet werden muss, wobei die Werte als UDINT‑Konstanten vorliegen.
- **Test‑ und Simulationsumgebungen**: Auswahl unterschiedlicher Testmuster oder Simulationsdaten über Ereignisse, ohne die Datenwerte zur Laufzeit ändern zu müssen.
- **Redundante Wertbereitstellung**: Bereitstellung von sechs verschiedenen vordefinierten Werten, die je nach Prozessschritt über Events angefordert werden.

## Vergleich mit ähnlichen Bausteinen

- **AUDI_AUI_MUX_3_VAL**: Eine kleinere Variante mit nur drei Eingängen, die analog funktioniert. `AUDI_AUI_MUX_6_VAL` erweitert diese um weitere drei Kanäle und ist daher für Anwendungen mit mehr Auswahlmöglichkeiten geeignet.
- **Generische Multiplexer‑Bausteine**: Ohne spezifische Datenumwandlung, meist mit numerischem Auswahl‑Eingang (z. B. INT), während dieser Baustein die Auswahl ausschließlich über Ereignisse vornimmt und die Werte in einen speziellen Adaptertyp umwandelt.
- **Bausteine mit Daten‑Ausgängen**: Im Gegensatz zu Bausteinen, die einen skalaren Datenwert ausgeben, liefert dieser FB einen Adapter, was ihn für die Kommunikation mit anderen Adapter‑basierten Komponenten prädestiniert.

## Fazit

Der Funktionsblock `AUDI_AUI_MUX_6_VAL` bietet eine flexible und kompakte Lösung zur ereignisgesteuerten Auswahl aus sechs UDINT‑Werten, die als AUDI‑Adapter nach außen geführt werden. Durch den modularen Aufbau als Subapplikation und die Wiederverwendung erprobter Komponenten ist er zuverlässig und gut in bestehende 4diac‑Projekte integrierbar. Die Kombination aus Ereignis‑Multiplexer, Datenkonvertierung und Adapter‑Multiplexer erfüllt die Anforderungen vieler Automatisierungs- und Steuerungsszenarien, ohne dass zusätzliche Logik erforderlich ist.