# A2X_CLIENT_2_0

![A2X_CLIENT_2_0](./A2X_CLIENT_2_0.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock **A2X_CLIENT_2_0** dient dazu, zwei binäre Signale eines A2X-Adapters (UP und DOWN) als OPC-UA-Write über einen CLIENT_2_0-Baustein an ein entferntes System zu übertragen. Die Signale werden dabei jeweils über einen E_D_FF-Flipflop zwischengespeichert, sodass auch bei kurzen Ereignissen ein zuverlässiges Senden gewährleistet ist. Der Baustein kapselt die komplette Kommunikationslogik und bietet eine einfache Schnittstelle für die Anbindung von binären Prozessdaten an ein OPC-UA-Netzwerk.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ   | Kommentar                                   |
|----------|-------|---------------------------------------------|
| INIT     | EInit | Initialisierung des Bausteins (initiiert Verbindungsaufbau) |

### **Ereignis-Ausgänge**

| Ereignis | Typ   | Kommentar                                   |
|----------|-------|---------------------------------------------|
| INITO    | EInit | Bestätigung der erfolgreichen Initialisierung |
| CNF      | Event | Bestätigung, dass die Daten gesendet wurden   |

### **Daten-Eingänge**

| Name | Typ     | Kommentar                                   |
|------|---------|---------------------------------------------|
| QI   | BOOL    | Steuersignal für die Aktivierung des Bausteins (Quality Indicator) |
| ID   | WSTRING | Adress- oder Identifikationsinformation für die OPC-UA-Verbindung |

### **Daten-Ausgänge**

| Name   | Typ     | Kommentar                                   |
|--------|---------|---------------------------------------------|
| QO     | BOOL    | Qualitätssignal – zeigt den Betriebszustand des Bausteins an |
| STATUS | WSTRING | Statusmeldung oder Fehlercode der Kommunikation |

### **Adapter**

| Socket | Typ                               | Kommentar                                   |
|--------|-----------------------------------|---------------------------------------------|
| IN     | adapter::types::unidirectional::A2X | Bietet die Eingänge UP und DOWN (BOOL) sowie die Ereignisse E_UP und E_DOWN |

## Funktionsweise

Der Baustein nutzt intern zwei E_D_FF-Flipflops (UP und DOWN) sowie einen CLIENT_2_0-Baustein für die OPC-UA-Kommunikation. Die Ereignisse `E_UP` und `E_DOWN` vom Adapter setzen jeweils das zugehörige Flipflop, wobei der aktuelle Wert von `UP` bzw. `DOWN` als Dateneingang dient. Die Ausgänge `Q` der Flipflops werden als Datenwerte `SD_1` und `SD_2` an den CLIENT_2_0 übergeben. Sobald eines der Flipflops einen neuen Wert erfasst hat, wird ein `REQ`-Ereignis an den CLIENT_2_0 ausgelöst, der daraufhin die beiden Werte als OPC-UA-Write an das konfigurierte Ziel sendet.

Die Initialisierung (`INIT`) wird direkt an den CLIENT_2_0 weitergeleitet. Nach erfolgreichem Verbindungsaufbau antwortet der CLIENT_2_0 mit `INITO`. Nach jedem Sendevorgang wird ein `CNF`-Ereignis emittiert. Die Ausgänge `QO` und `STATUS` spiegeln die entsprechenden Ausgänge des CLIENT_2_0 wider.

## Technische Besonderheiten

- **Pufferung durch E_D_FF**: Jedes Binärsignal wird in einem flankengesteuerten Flipflop gespeichert. Dadurch wird verhindert, dass ein kurzes Ereignis verloren geht, falls der CLIENT_2_0 gerade nicht bereit ist.
- **Einheitliche Datenübertragung**: Die beiden Bool-Werte werden in einem einzigen OPC-UA-Write übertragen, was die Netzwerklast reduziert und die Konsistenz der Daten gewährleistet.
- **Direkte Durchreichung**: Die Status- und Qualitätssignale (`QO`, `STATUS`) werden unverändert vom CLIENT_2_0 übernommen, sodass der Anwender den Kommunikationsstatus genau verfolgen kann.
- **Ereignisgesteuerte Aktualisierung**: Es werden nur dann Daten gesendet, wenn sich der Wert eines der Binärsignale ändert – kein kontinuierliches Polling erforderlich.

## Zustandsübersicht

Der Baustein besitzt keine explizit modellierten Zustände, das Verhalten wird durch die internen Komponenten bestimmt:

- **Initialisierungsphase**: Nach dem `INIT`-Ereignis versucht der CLIENT_2_0 eine Verbindung zum Zielsystem aufzubauen. Erfolgreicher Abschluss wird durch `INITO` signalisiert.
- **Bereitschaft**: Nach erfolgreicher Initialisierung wartet der Baustein auf Ereignisse an den Adaptereingängen.
- **Sendeoperation**: Tritt ein Ereignis `E_UP` oder `E_DOWN` auf, wird der aktuelle Wert übernommen und ein `REQ` an den CLIENT_2_0 gesendet. Nach Abschluss des Schreibvorgangs wird `CNF` ausgegeben.
- **Fehlerfall**: Wenn die Verbindung verloren geht oder der CLIENT_2_0 einen Fehler meldet, wird dies über `STATUS` und ggfs. `QO` wiedergegeben.

## Anwendungsszenarien

- **Fernsteuerung**: Übertragung von zwei Schaltzuständen (z. B. Auf/Ab, Ein/Aus) von einer SPS an eine übergeordnete Leitstelle via OPC-UA.
- **Datenbrücke**: Anbindung eines A2X-Adapters an ein OPC-UA-basiertes Überwachungssystem, ohne dass separate Kommunikationsbausteine verdrahtet werden müssen.
- **Zuverlässige Ereignisübertragung**: Einsatz in Umgebungen, in denen kurze Impulse oder Flanken sicher erfasst und übertragen werden müssen, z. B. bei Zählern oder Positionssignalen.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einem direkten Einsatz eines `CLIENT_2_0` ohne vorgeschaltete Pufferung bietet `A2X_CLIENT_2_0` eine integrierte Ereignisbehandlung. Ein einfacher `CLIENT_2_0` erfordert eine manuelle Zuordnung von Ereignissen und Datenwerten, während dieser Baustein die komplette Logik kapselt und dem Anwender eine kompakte, wiederverwendbare Schnittstelle bietet. Zudem werden durch die Flipflops Datenverluste durch zeitliche Verzögerungen zwischen Ereignis und Sendebereitschaft vermieden. Gegenüber einem generischen OPC-UA-Client zeichnet sich dieser Baustein durch die spezifische Anpassung an die A2X-Adapter-Semantik aus.

## Fazit

Der Funktionsblock **A2X_CLIENT_2_0** stellt eine robuste und komfortable Lösung dar, um zwei binäre Signale über OPC-UA zu übertragen. Durch die eingebaute Pufferung und die ereignisgesteuerte Verarbeitung werden auch zeitkritische Signale zuverlässig versendet. Die klare Schnittstelle und die Wiederverwendbarkeit machen ihn zu einem nützlichen Baustein für Automatisierungs- und Visualisierungsanwendungen, die eine OPC-UA-Anbindung erfordern.