# AX_E_PERMIT_4

![AX_E_PERMIT_4](./AX_E_PERMIT_4.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AX_E_PERMIT_4** dient zur bedingten Weitergabe von vier unabhängigen Ereigniskanälen. Er besitzt jeweils vier Ereignis-Eingänge (EI1–EI4) und vier Ereignis-Ausgänge (EO1–EO4). Die Durchschaltung der Ereignisse wird über einen einzigen Adapter-Eingang (PERMIT) gesteuert. Dieser Adapter liefert ein Freigabesignal (permit), das als Bedingung für die Weiterleitung aller Ereignisse wirkt. Der Baustein ist als generischer FB (Generic FB) ausgelegt und kann in der 4diac-IDE mittels des generischen Typs `GEN_AX_E_PERMIT` instanziiert werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Beschreibung |
|------|--------------|
| EI1  | Ereignis-Eingangskanal 1   |
| EI2  | Ereignis-Eingangskanal 2   |
| EI3  | Ereignis-Eingangskanal 3   |
| EI4  | Ereignis-Eingangskanal 4   |

### **Ereignis-Ausgänge**

| Name | Beschreibung |
|------|--------------|
| EO1  | Ereignis-Ausgangskanal 1   |
| EO2  | Ereignis-Ausgangskanal 2   |
| EO3  | Ereignis-Ausgangskanal 3   |
| EO4  | Ereignis-Ausgangskanal 4   |

### **Daten-Eingänge**

Der Baustein besitzt keine Daten-Eingänge.

### **Daten-Ausgänge**

Der Baustein besitzt keine Daten-Ausgänge.

### **Adapter**

| Name   | Typ                                   | Richtung | Beschreibung                                   |
|--------|---------------------------------------|----------|------------------------------------------------|
| PERMIT | adapter::types::unidirectional::AX    | Socket (Eingang) | Freigabebedingung für die Ereignisweiterleitung. |

## Funktionsweise

Der Baustein überwacht den über den Adapter `PERMIT` ankommenden Zustand. Solange das Signal den Wert „freigegeben“ (permit) hat, werden eingehende Ereignisse nahezu verzögerungsfrei auf den jeweils korrespondierenden Ausgang durchgeschaltet (EI1 → EO1, EI2 → EO2, …). Ist das Signal nicht aktiv, werden eingehende Ereignisse ignoriert und nicht an die Ausgänge weitergegeben.

Es findet keine Verarbeitung oder Umkodierung der Ereignisse statt – die Kopplung zwischen Eingang und Ausgang ist strikt 1:1. Die Bedingung wird global für alle vier Kanäle gleichzeitig ausgewertet; eine individuelle Freigabe pro Kanal ist nicht möglich.

## Technische Besonderheiten

- **Generischer FB**: Der Baustein ist als generischer Typ deklariert (`eclipse4diac::core::GenericClassName = 'GEN_AX_E_PERMIT'`). Dadurch kann er in der 4diac-IDE flexibel parametriert und an unterschiedliche Adaptertypen angepasst werden.
- **Adapter-Prinzip**: Die Freigabebedingung wird über einen unidirektionalen Adapter (Socket) realisiert. Dies erlaubt eine klare Trennung zwischen Ereignisfluss und Steuerfluss.
- **Keine Datenkanäle**: Es sind weder Daten-Eingänge noch Daten-Ausgänge vorhanden – die gesamte Logik reduziert sich auf reine Ereignisweiterleitung mit einer binären Bedingung.
- **Skalierbarkeit**: Durch die Verwendung von vier Kanälen eignet sich der Baustein für Anwendungen, bei denen mehrere unabhängige Ereignisströme gleichzeitig überwacht und gefiltert werden müssen.

## Zustandsübersicht

Da der Baustein keine explizite Zustandsmaschine besitzt, kann er als ereignisgesteuert mit internem Schwellwert (permit) beschrieben werden. Es gibt keine persistenten Zustände; die Funktionsweise ist rein kombinatorisch – das permit-Signal bestimmt unmittelbar, ob ein Ereignis durchgelassen wird. Eine Zustandsübersicht im herkömmlichen Sinne ist daher nicht erforderlich.

## Anwendungsszenarien

Typische Einsatzbereiche sind:

- **Steuerungssysteme**: Durchschaltung von Alarm- oder Statusereignissen nur bei erfolgreicher Freigabe (z.B. durch übergeordnete Sicherheitslogik).
- **Sicherheitsgerichtete Kommunikation**: Puffern oder Blockieren von Ereignisketten in Abhängigkeit von Betriebszuständen (z.B. Stillstand, Not-Aus).
- **Test- und Simulationsumgebungen**: Gezieltes Ein-/Ausschalten von Ereignispfaden zu Testzwecken.
- **Modulare Automatisierung**: Einheitliche Ereignisweiterleitung in größeren Funktionsblöcken, wenn mehrere Signalquellen nur bei aktiver Bedingung durchgereicht werden dürfen.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfachen Weiterleitungsbausteinen wie `E_PERMIT` (der nur einen Kanal besitzt) bietet `AX_E_PERMIT_4` eine Mehrkanalausführung mit gemeinsamer Freigabe. Im Gegensatz zu Bausteinen ohne Freigabebedingung (z.B. `E_SPLIT` oder `E_MERGE`) wird hier eine zusätzliche Steuerebene hinzugefügt, die die Ereignisübertragung konditioniert. Gegenüber einem Aufbau aus vier separaten `E_PERMIT`-Blöcken reduziert dieser FB die Anzahl benötigter Instanzen und erleichtert die Synchronisierung der Freigabe.

## Fazit

Der Funktionsblock **AX_E_PERMIT_4** ist ein kompaktes und effizientes Werkzeug zur bedingten Weitergabe von vier unabhängigen Ereignisströmen. Durch die Integration eines Adapters für das Freigabesignal und die generische Auslegung eignet er sich besonders für modulare und sicherheitsbewusste Automatisierungslösungen. Seine einfache Struktur ohne Datenpfade erlaubt eine klare, zuverlässige Ereignissteuerung und kann leicht in bestehende 4diac-Projekte integriert werden.
