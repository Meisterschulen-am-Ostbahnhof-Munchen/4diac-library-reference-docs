# E_PERMIT_3


![E_PERMIT_3_network](./E_PERMIT_3_network.svg)

![E_PERMIT_3](./E_PERMIT_3.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **E_PERMIT_3** ist eine Subapplikation (SubApp), die drei unabhängige Ereignis-Freigabe-Gates (E_PERMIT) in einer kompakten Einheit bündelt. Sie dient dazu, drei parallele Ereigniskanäle wahlweise durchzuschalten oder zu blockieren – gesteuert durch ein gemeinsames Freigabesignal. Die SubApp ist geeignet für Anwendungen, in denen mehrere Ereignisse synchron freigegeben oder gesperrt werden müssen, ohne dass einzelne Instanzen separat konfiguriert werden müssen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name  | Datentyp | Beschreibung                     |
|-------|----------|----------------------------------|
| EI1   | Event    | Ereigniseingang für Kanal 1      |
| EI2   | Event    | Ereigniseingang für Kanal 2      |
| EI3   | Event    | Ereigniseingang für Kanal 3      |

### **Ereignis-Ausgänge**

| Name  | Datentyp | Beschreibung                     |
|-------|----------|----------------------------------|
| EO1   | Event    | Ereignisausgang für Kanal 1      |
| EO2   | Event    | Ereignisausgang für Kanal 2      |
| EO3   | Event    | Ereignisausgang für Kanal 3      |

### **Daten-Eingänge**

| Name   | Datentyp | Beschreibung                     |
|--------|----------|----------------------------------|
| PERMIT | BOOL     | Zentrales Freigabesignal für alle drei Kanäle |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

Keine vorhanden.

## Funktionsweise

Die SubApp **E_PERMIT_3** enthält intern drei Instanzen des standardisierten Funktionsblocks `iec61499::events::E_PERMIT`. Jeder dieser Bausteine fungiert als Ereignis-Gate: Er lässt ein ankommendes Ereignis (EI) nur dann zum Ausgang (EO) durch, wenn das zugehörige Freigabesignal (PERMIT) den Wert `TRUE` (bzw. `1`) besitzt. Ist das Freigabesignal `FALSE`, wird das Ereignis verworfen.

Die drei internen E_PERMIT-Bausteine sind parallel geschaltet und arbeiten völlig unabhängig voneinander – die Freigabebedingung wird jedoch über den externen Dateneingang `PERMIT` allen drei Bausteinen gleichzeitig zugeführt. Dadurch gilt die Freigabe global: Entweder werden alle drei Ereigniskanäle durchgeschaltet oder alle drei blockiert.

Die Ereignisverbindungen verbinden jeweils die externen Eingänge `EI1`, `EI2`, `EI3` mit den internen Eingängen der E_PERMIT-Instanzen, und die internen Ausgänge mit den externen Ausgängen `EO1`, `EO2`, `EO3`. Eine zusätzliche interne Logik oder zeitliche Verzögerung existiert nicht.

## Technische Besonderheiten

- **Wiederverwendbarkeit:** Als SubApp ist E_PERMIT_3 als eigenständiger Baustein in Bibliotheken ablegbar und kann in verschiedenen Projekten mehrfach instanziiert werden.
- **Skalierbarkeit:** Durch die modulare Struktur lässt sich die Anzahl der Kanäle einfach erweitern, indem weitere E_PERMIT-Instanzen hinzugefügt werden.
- **Lizenz:** Der Baustein ist unter der Eclipse Public License 2.0 (EPL-2.0) veröffentlicht und kann frei verwendet und angepasst werden.
- **Kompatibilität:** Er folgt dem Standard IEC 61499-2 und ist damit in 4diac-IDE und anderen kompatiblen Systemen einsetzbar.

## Zustandsübersicht

Da E_PERMIT_3 eine reine Zusammenstellung von E_PERMIT-Bausteinen ist, besitzt die SubApp selbst keinen internen Zustandsautomaten. Das Verhalten wird ausschließlich durch die enthaltenen E_PERMIT-Instanzen bestimmt.

Jede E_PERMIT-Instanz besitzt intern die folgenden Zustände:

| Zustand | Beschreibung                                                                 |
|---------|------------------------------------------------------------------------------|
| IDLE    | Warten auf ein Ereignis am Eingang EI.                                       |
| PASS    | Ereignis wird durchgereicht, sofern PERMIT = TRUE.                           |
| BLOCK   | Ereignis wird verworfen, sofern PERMIT = FALSE.                              |

Die Umschaltung zwischen PASS und BLOCK erfolgt unmittelbar, sobald sich der Wert von PERMIT ändert. Ereignisse, die während eines Zustandswechsels eintreffen, werden entsprechend dem aktuellen PERMIT-Wert behandelt.

## Anwendungsszenarien

- **Mehrkanalige Sicherheitsfreigaben:** In Maschinensteuerungen müssen oft mehrere unabhängige Signale (z. B. Not-Halt, Schutztür, Startsignal) gleichzeitig freigegeben werden, bevor ein Prozess startet. E_PERMIT_3 kann als gemeinsames Gate dienen.
- **Synchronisierte Ereignisweiterleitung:** Wenn mehrere parallele Ereignisströme nur bei einer gemeinsamen Bedingung an nachgelagerte Logik weitergeleitet werden sollen.
- **Test- und Simulationsumgebungen:** Zur gezielten Unterdrückung oder Freischaltung mehrerer Ereignisquellen in einem System.

## Vergleich mit ähnlichen Bausteinen

- **E_PERMIT (einzeln):** Bietet nur einen Kanal. Für mehrere Kanäle müssen mehrere Instanzen verdrahtet werden, wobei die Freigabesignale separat oder über gemeinsame Variablen verbunden werden müssen. E_PERMIT_3 vereinfacht dies durch eine vorgefertigte 3‑Kanal-Lösung mit zentralem Freigabeeingang.
- **E_SWITCH:** Ein Ereignisverteiler, der Ereignisse an verschiedene Ausgänge weiterleitet, je nach Datenwert. E_PERMIT_3 hingegen blockiert oder lässt Ereignisse unverändert passieren, ohne eine Umleitung vorzunehmen.
- **E_REND:** Ein Ereignis-Gate, das nur bei steigender Flanke eines Bool-Signals durchlässig wird. E_PERMIT_3 verwendet stattdessen den aktuellen Pegel (Level) des Freigabesignals.

## Fazit

Der Funktionsblock **E_PERMIT_3** ist eine einfach strukturierte, aber praxisorientierte SubApp zur effizienten Steuerung von drei Ereigniskanälen über eine gemeinsame Bool-Freigabe. Er reduziert den Verdrahtungsaufwand, erhöht die Übersichtlichkeit in Anlagen und ist dank seiner klaren Semantik vielseitig einsetzbar. Die Einhaltung des IEC-61499-Standards und die offene Lizenz machen ihn zu einem nützlichen Baustein für industrielle Automatisierungsprojekte und Lehrmaterialien gleichermaßen.
