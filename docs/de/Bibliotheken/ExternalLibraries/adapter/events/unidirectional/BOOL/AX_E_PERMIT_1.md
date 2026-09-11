# AX_E_PERMIT_1

![AX_E_PERMIT_1](./AX_E_PERMIT_1.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `AX_E_PERMIT_1` ist ein generischer Baustein zur kontrollierten Weiterleitung von Ereignissen. Er besitzt einen Ereignis-Eingang und einen Ereignis-Ausgang. Die Weitergabe eines Ereignisses von `EI1` zu `EO1` erfolgt nur dann, wenn die über einen Adapter definierte Bedingung (Freigabe) erfüllt ist. Der Baustein implementiert damit eine „permissive Propagation“: Ereignisse werden nur bei aktiver Erlaubnis durchgeschaltet, andernfalls verworfen. Die Bedingung wird über einen Adapter vom Typ `adapter::types::unidirectional::AX` extern bereitgestellt und kann je nach Anwendung flexibel gestaltet werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name  | Typ   | Kommentar                   |
|-------|-------|-----------------------------|
| `EI1` | Event | Ereignis-Eingangskanal 1    |

### **Ereignis-Ausgänge**

| Name  | Typ   | Kommentar                   |
|-------|-------|-----------------------------|
| `EO1` | Event | Ereignis-Ausgangskanal 1    |

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Name     | Typ                                   | Kommentar                             |
|----------|---------------------------------------|---------------------------------------|
| `PERMIT` | `adapter::types::unidirectional::AX`  | Adapter-Eingang für Freigabebedingung (Socket) |

## Funktionsweise

Der Baustein arbeitet rein ereignisbasiert. Er wartet auf ein Ereignis am Eingang `EI1`. Sobald ein Ereignis eintrifft, wird der Zustand des Adapters `PERMIT` ausgewertet. Liefert der Adapter eine „Erlaubnis“ (Freigabe), wird das Ereignis unmittelbar am Ausgang `EO1` weitergegeben. Ist die Bedingung nicht erfüllt, wird das Ereignis ignoriert und nicht weitergeleitet.

Die genaue Logik zur Bestimmung der Freigabe ist durch den angeschlossenen Adapter bestimmt. Der Adapter `AX` ist als unidirektionaler Typ definiert, d.h. er liefert ausschließlich eine boolesche oder vergleichbare Freigabesignal an den Funktionsblock. Der FB selbst enthält keine Datenverarbeitungslogik; er dient ausschließlich als steuerbares Ereignistor.

## Technische Besonderheiten

- **Generischer Funktionsblock**: Der Baustein ist als generischer Typ (`GEN_AX_E_PERMIT`) implementiert und kann über den Adapter an verschiedene Freigabelogiken angepasst werden.
- **Keine Datenschnittstellen**: Der FB besitzt weder Daten-Eingänge noch Daten-Ausgänge. Die Kommunikation erfolgt ausschließlich über Ereignisse und den Adapter.
- **Unidirektionaler Adapter**: Der Adapter `PERMIT` ist als Socket deklariert, typisiert mit `adapter::types::unidirectional::AX`. Dadurch ist eine klare Trennung zwischen Freigabesignal und Ereignisfluss gegeben.
- **Einfache Ereignisweiterleitung**: Die Implementierung folgt dem Muster einer bedingten Ereignisübertragung, was den Baustein leicht in sicherheitsgerichtete oder ablaufsteuernde Systeme integrierbar macht.

## Zustandsübersicht

Da der Baustein keine eigenen Zustände speichert, sondern jede Ereignisinstanz unabhängig prüft, lässt sich keine klassische Zustandsmaschine darstellen. Die Funktionsweise ist rein ereignisorientiert:

- **Warte auf Ereignis**: Kein Ereignis am Eingang, keine Aktivität.
- **Ereignis eingetroffen**: Prüfung der Adapterbedingung.
  - Bedingung erfüllt → Weiterleitung über `EO1`.
  - Bedingung nicht erfüllt → Verwerfen des Ereignisses.

Somit existiert lediglich ein impliziter, ereignisabhängiger Ablauf ohne dauerhafte Zustände.

## Anwendungsszenarien

Der Baustein eignet sich für alle Systeme, in denen Ereignisse nur bei Vorliegen einer externen Freigabe wirksam werden dürfen. Beispiele:

- **Sicherheitskreise**: Not-Aus-Signale werden nur dann an die Steuerung weitergegeben, wenn eine Betriebsfreigabe aktiv ist.
- **Zugangssteuerungen**: Türen oder Schranken lassen nur bei erteilter Zugangsberechtigung (über Adapter) ein Ereignis (z.B. „Schließen“) zu.
- **Produktionsanlagen**: Ereignisse wie „Start“ oder „Stopp“ werden nur bei freigegebenem Prozessmodul durchgeschaltet.
- **Testumgebungen**: Gezieltes Filtern von Ereignissen während des Testbetriebs durch einen separaten Freigabeadapter.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einem einfachen Ereignis-Durchschaltbaustein (z.B. `E_PERMIT`) bietet `AX_E_PERMIT_1` die Möglichkeit, die Freigabebedingung über einen Adapter flexibel extern zu definieren. Dadurch werden fest verdrahtete Bedingungslaogiken zu Gunsten einer modularen, wiederverwendbaren Architektur aufgelöst.

Gegenüber Bausteinen mit Datenfiltern (z.B. `E_CYCLE` oder `E_SWITCH`) fokussiert dieser FB ausschließlich auf die Ereignisfreigabe, ohne Daten zu manipulieren oder zeitliche Aspekte zu berücksichtigen. Der unidirektionale Adapter vereinfacht die Anbindung an vorhandene Freigabesignale, ohne zusätzliche Verdrahtungsaufwand für Rückkanäle.

## Fazit

`AX_E_PERMIT_1` ist ein schlanker, generischer Funktionsblock zur bedingten Ereignisweiterleitung. Durch die Verwendung eines Adapters bleibt die Freigabelogik austauschbar und anpassbar. Die klare Beschränkung auf Ereignisse und den Adapter macht den Baustein zu einem vielseitigen Werkzeug für sicherheitsrelevante und steuerungstechnische Anwendungen, bei denen eine Erlaubnisprüfung vor der Weitergabe von Ereignissen erforderlich ist. Seine einfache Struktur und die generische Konfiguration ermöglichen eine schnelle Integration in bestehende 4diac-Umgebungen.
