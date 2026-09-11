# A2X_PUBLISH_2

![A2X_PUBLISH_2](./A2X_PUBLISH_2.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock `A2X_PUBLISH_2` ist ein zusammengesetzter Baustein (Composite FB), der dazu dient, zwei boolesche Werte eines A2X-Adapters über ein Netzwerk zu publizieren. Er basiert auf dem Standard-Funktionsblock `PUBLISH_2` und ergänzt diesen um eine Adapter-Schnittstelle vom Typ `adapter::types::unidirectional::A2X`. Durch die Verwendung von flankengetriggerten Speichern (`E_D_FF`) werden Daten nur dann gesendet, wenn sich einer der beiden Werte ändert, wodurch unnötige Netzwerklast vermieden wird.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Datentyp | Beschreibung |
|----------|----------|--------------|
| `INIT`   | `EInit`  | Initialisierung des Bausteins. Parameter `QI` und `ID` werden übergeben und an den internen `PUBLISH_2` weitergeleitet. |

### **Ereignis-Ausgänge**

| Ereignis | Datentyp | Beschreibung |
|----------|----------|--------------|
| `INITO`  | `EInit`  | Bestätigung der erfolgreichen Initialisierung. Wird ausgelöst, wenn der `PUBLISH_2` die Initialisierung abgeschlossen hat. |
| `CNF`    | `Event`  | Bestätigung, dass Daten erfolgreich gesendet wurden. Ausgelöst nach jedem Sendevorgang. |

### **Daten-Eingänge**

| Name  | Datentyp | Beschreibung |
|-------|----------|--------------|
| `QI`  | `BOOL`   | Aktivierungssignal für den Baustein. Bei `FALSE` wird keine Kommunikation durchgeführt. |
| `ID`  | `WSTRING`| Kennung (z.B. IP-Adresse und Port) für den Publikationskanal. Wird an `PUBLISH_2` übergeben. |

### **Daten-Ausgänge**

| Name     | Datentyp | Beschreibung |
|----------|----------|--------------|
| `QO`     | `BOOL`   | Status der letzten Operation (siehe `PUBLISH_2`): `TRUE` bei Erfolg, `FALSE` bei Fehler. |
| `STATUS` | `WSTRING`| Detaillierte Fehlermeldung oder Statusinformation. |

### **Adapter**

| Typ | Richtung | Name | Beschreibung |
|-----|----------|------|--------------|
| `adapter::types::unidirectional::A2X` | Socket | `IN` | Empfängt die beiden booleschen Werte `UP` und `DOWN` sowie die zugehörigen Ereignisse `E_UP` und `E_DOWN` von einer übergeordneten Anwendung. |

## Funktionsweise

Der Baustein verwendet intern drei Funktionsblöcke:

- `E_D_FF_UP` und `E_D_FF_DOWN`: flankengetriggerte D-Flip-Flops (Typ `iec61499::events::E_D_FF`), die jeweils einen der beiden Eingangswerte ( `IN.UP` bzw. `IN.DOWN` ) puffern und bei einem Ereignis ( `E_UP` bzw. `E_DOWN` ) einen Ausgangsimpuls erzeugen.
- `PUBLISH_2`: der eigentliche Netzwerk-Publisher, der die gepufferten Werte an ein Ziel sendet.

Ablauf:

1. **Initialisierung**: Über das Ereignis `INIT` werden `QI` und `ID` an den `PUBLISH_2` übergeben und dessen Initialisierung gestartet. Nach erfolgreicher Initialisierung wird `INITO` ausgelöst.
2. **Datenänderung**: Wenn am Adapter `IN` entweder das Ereignis `E_UP` oder `E_DOWN` eintrifft, wird das entsprechende Flip-Flop mit dem aktuellen Wert ( `IN.UP` bzw. `IN.DOWN` ) übernommen. Dieses Flip-Flop erzeugt seinerseits ein Ereignis an seinem Ausgang `EO`, das mit dem `REQ`-Eingang des `PUBLISH_2` verbunden ist.
3. **Senden**: Der `PUBLISH_2` sendet dann die beiden gespeicherten Werte `SD_1` (vom Flip-Flop `UP`) und `SD_2` (vom Flip-Flop `DOWN`) als zusammengehörigen Datensatz über das Netzwerk. Nach erfolgreichem Senden wird das Ereignis `CNF` ausgelöst und die Statusausgänge `QO` und `STATUS` aktualisiert.

Hinweis: Die beiden Flip-Flops sind unabhängig voneinander. Wenn nur einer der beiden Werte geändert wird, wird trotzdem ein komplettes Datenpaket mit beiden aktuellen Werten gesendet.

## Technische Besonderheiten

- Der Baustein ist ein **Composite FB** und kapselt die Logik, sodass der Anwender lediglich die Adapter-Schnittstelle und die Parameter `QI`/`ID` verwenden muss.
- Die Verwendung der **E_D_FF**-Bausteine sorgt dafür, dass Daten nur bei **Signalflanken** gesendet werden – es wird also keine kontinuierliche Übertragung durchgeführt, sondern nur bei Änderungen.
- Der Baustein ist für **unidirektionale** Kommunikation ausgelegt (nur Senden, kein Empfangen). Der Socket `IN` liefert die Daten, die publiziert werden.
- Der interne `PUBLISH_2` basiert auf dem Standard der IEC 61499 und unterstützt verschiedene Netzwerkprotokolle (z.B. UDP, TCP – abhängig von der Implementierung).
- Die Software ist unter der **Eclipse Public License 2.0** lizenziert und wurde für die 4diac-IDE entwickelt.

## Zustandsübersicht

Der Baustein hat keinen expliziten Zustandsautomaten, sondern nutzt die internen Zustände des `PUBLISH_2`. Exemplarisch:

- **Initialisierung läuft**: Nach dem `INIT`-Ereignis wartet der `PUBLISH_2` auf die Bestätigung durch das `INITO`-Ereignis.
- **Bereit**: Nach der Initialisierung kann der Baustein Daten senden. Solange `QI` den Wert `TRUE` hat, werden eingehende E_UP/E_DOWN-Ereignisse verarbeitet.
- **Senden aktiv**: Während eines Sendevorgangs werden weitere Ereignisse gepuffert (durch die Flip-Flops), bis der aktuelle Vorgang abgeschlossen ist.
- **Fehlerzustand**: Bei Fehlern (z.B. Netzwerk nicht erreichbar) wird `STATUS` entsprechend gesetzt und `QO` auf `FALSE` gesetzt. Nach Behebung des Fehlers kann der Baustein über `INIT` erneut initialisiert werden.

## Anwendungsszenarien

- **Maschinensteuerung**: Veröffentlichung von zwei Status-Bits (z.B. „Bewegung oben/unten“) an eine übergeordnete Leitebene, wobei nur bei Änderungen gesendet wird.
- **Gebäudeautomation**: Publizieren von Schaltzuständen (z.B. Licht an/aus) über ein Netzwerk an ein Visualisierungssystem.
- **Industrie 4.0 / IoT**: Integration in verteilte Steuerungssysteme, bei denen eine minimale Netzwerklast gewünscht ist.

## Vergleich mit ähnlichen Bausteinen

- **`A2X_PUBLISH`** (ohne Suffix „2“): Publiziert nur einen einzelnen BOOL-Wert über einen Adapter. `A2X_PUBLISH_2` erweitert dies auf zwei Werte, was für Anwendungen mit zwei zusammengehörigen Signalen (z.B. Richtung und Status) nützlich ist.
- **`PUBLISH`** (Standard): Bietet eine generische Publikationsfunktion für mehrere Datenwerte, erfordert aber eine manuelle Fehlerbehandlung und Ereignissteuerung. `A2X_PUBLISH_2` kapselt diese Logik und bietet eine komfortablere Adapter-Anbindung.
- **`PUBLISH_2`** (Standard): Kann zwei Datenwerte senden, benötigt aber zusätzliche Logik, um nur bei Änderungen zu senden. Der vorliegende Baustein ergänzt diese Funktionalität durch die integrierten Flip-Flops.

## Fazit

`A2X_PUBLISH_2` ist ein spezialisierter Baustein für die effiziente Veröffentlichung von zwei booleschen Signalen über ein Netzwerk. Durch die Kombination von Adapter, flankengetriggerten Speichern und dem Standard-Publisher wird eine robuste und einfach zu integrierende Lösung für verteilte Steuerungssysteme bereitgestellt. Der Baustein reduziert die Netzwerkkommunikation auf tatsächliche Änderungen und erleichtert die Anbindung an höhere Ebenen. Dank der klaren Schnittstellen und der integrierten Statusbehandlung ist er für eine Vielzahl industrieller Anwendungen geeignet.