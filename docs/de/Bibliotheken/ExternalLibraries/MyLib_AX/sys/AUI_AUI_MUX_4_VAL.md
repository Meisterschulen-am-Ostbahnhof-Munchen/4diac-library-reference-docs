# AUI_AUI_MUX_4_VAL


![AUI_AUI_MUX_4_VAL_network](./AUI_AUI_MUX_4_VAL_network.svg)

![AUI_AUI_MUX_4_VAL](./AUI_AUI_MUX_4_VAL.svg)

* * * * * * * * * *
## Einleitung
Der Funktionsblock **AUI_AUI_MUX_4_VAL** ist ein 4‑Wege‑Multiplexer für AUI/UINT‑Werte. Er wählt über ein Ereignis an einem der vier Eingänge einen der vier Datenwerte (val1…val4) aus und stellt diesen als AUI‑Adapter‑Ausgang bereit. Die Auswahl erfolgt ereignisgesteuert und wird durch die Kombination eines Ereignis‑Multiplexers und eines Adapter‑Multiplexers realisiert. Zusätzlich werden die Eingangswerte über interne `initval_AUI`‑Bausteine initialisiert, um ein definiertes Startverhalten zu gewährleisten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
| Ereignis | Kommentar |
|----------|-----------|
| EI1 | Event zur Auswahl von val1 |
| EI2 | Event zur Auswahl von val2 |
| EI3 | Event zur Auswahl von val3 |
| EI4 | Event zur Auswahl von val4 |

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
| Daten | Typ | Kommentar |
|-------|-----|-----------|
| val1 | UINT | Initialer Ausgabewert bei EI1 |
| val2 | UINT | Initialer Ausgabewert bei EI2 |
| val3 | UINT | Initialer Ausgabewert bei EI3 |
| val4 | UINT | Initialer Ausgabewert bei EI4 |

### **Daten-Ausgänge**
Keine (Ausgabe erfolgt über den Adapter).

### **Adapter**
| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| OUT | `adapter::types::unidirectional::AUI` | Ausgang | Ausgewählter AUI-Adapter Output |

## Funktionsweise
Der Baustein verwendet intern drei Arten von Bausteinen:
- **`AUI_MUX_4`**: Ein Ereignis‑Multiplexer, der die ankommenden Events (EI1…EI4) entgegennimmt und ein entsprechendes Auswahlsignal an den Adapter‑Multiplexer weiterleitet.
- **`AUI_AUI_MUX_4`**: Ein Adapter‑Multiplexer, der anhand des erhaltenen Auswahlsignals einen der vier Eingänge (IN1…IN4) auf den Ausgang (OUT) schaltet.
- **Vier `initval_AUI`‑Instanzen**: Diese setzen den jeweils übergebenen UINT‑Wert (val1…val4) in einen AUI‑Adapter‑Wert um und stellen diesen am jeweiligen Eingang des Adapter‑Multiplexers bereit.

Wird ein Ereignis an einem der Eingänge ausgelöst (z. B. EI1), so aktiviert der `AUI_MUX_4` die entsprechende Auswahlleitung. Gleichzeitig werden die aktuellen Werte der Daten‑Eingänge über die `initval_AUI`‑Bausteine an die Eingänge des `AUI_AUI_MUX_4` geführt. Der Adapter‑Multiplexer übernimmt dann den passenden Wert an seinem Ausgang `OUT`. Somit wird der Wert, der zum Zeitpunkt des Ereignisses an `val1` anliegt, als AUI‑Wert ausgegeben.

## Technische Besonderheiten
- **Verwendung von AUI‑Adaptern**: Die Ausgabe ist als unidirektionaler AUI‑Adapter ausgeführt, was eine einfache Kopplung an weitere Bausteine mit AUI‑Schnittstelle ermöglicht.
- **Zweistufige Multiplex‑Logik**: Die Trennung von Ereignis‑Auswahl (durch `AUI_MUX_4`) und Daten‑Auswahl (durch `AUI_AUI_MUX_4`) erlaubt eine klare Struktur und erleichtert die Erweiterung auf mehr Kanäle.
- **Initialisierung**: Jeder Eingangswert wird über eine `initval_AUI`‑Instanz initialisiert. Dadurch ist gewährleistet, dass bereits vor dem ersten Ereignis definierte Werte an den Multiplexer‑Eingängen anliegen.
- **Reine Ereignissteuerung**: Es gibt keinen internen Zustandsspeicher; die aktive Auswahl wird eindeutig durch das zuletzt empfangene Ereignis bestimmt.

## Zustandsübersicht
Der Baustein besitzt keinen expliziten internen Zustand im Sinne einer Zustandsmaschine. Die aktive Auswahl wird ausschließlich durch das zuletzt empfangene Ereignis (EI1…EI4) bestimmt. Nach einem Reset oder einer Initialisierung ist kein Wert aktiv – erst ein ankommendes Ereignis legt den auszugebenden Wert fest. Daher kann man von einem ereignisgesteuerten Auswahlverhalten ohne persistenten Zustand sprechen.

## Anwendungsszenarien
- **Parametrisierung**: Auswahl unterschiedlicher Konfigurationswerte (z. B. Geschwindigkeit, Druck, Temperatur) in einer Steuerung über einzelne Triggersignale.
- **Datenumschaltung**: Umschalten zwischen verschiedenen Datenquellen (z. B. Sensoren) bei Bedarf.
- **Test- und Simulationsumgebungen**: Gezieltes Einspielen von Testwerten über die Events.
- **Adapter‑basierte Architekturen**: Einsatz in Systemen, die auf AUI‑Adaptern für die Kommunikation zwischen Funktionsblöcken basieren.

## Vergleich mit ähnlichen Bausteinen
Gegenüber einem einfachen Daten‑Multiplexer (z. B. mit `SELECT`‑Eingang) bietet dieser Baustein eine ereignisgesteuerte Auswahl, was in Echtzeit‑Systemen oft vorteilhaft ist, da keine aktive Polling‑Logik benötigt wird. Im Vergleich zu einem Multiplexer ohne `initval`-Unterstützung wird hier eine definierte Initialisierung der Eingänge gewährleistet. Die Verwendung von AUI‑Adaptern auf der Ausgabeseite macht den Baustein besonders für Systeme geeignet, die bereits auf AUI‑Kommunikation setzen.

## Fazit
Der **AUI_AUI_MUX_4_VAL** ist ein flexibler und klar strukturierter 4‑Kanal‑Multiplexer für AUI‑Werte. Durch die Kombination eines Ereignis‑Multiplexers mit einem Adapter‑Multiplexer und der Integration von Initialisierungs‑Bausteinen bietet er eine robuste und erweiterbare Lösung zur Auswahl von Datenwerten. Die ereignisgesteuerte Arbeitsweise macht ihn ideal für Automatisierungsanwendungen, bei denen schnelle und deterministische Umschaltungen erforderlich sind. Die Dokumentation und die klare Schnittstellenstruktur ermöglichen eine einfache Integration in bestehende Projekte.