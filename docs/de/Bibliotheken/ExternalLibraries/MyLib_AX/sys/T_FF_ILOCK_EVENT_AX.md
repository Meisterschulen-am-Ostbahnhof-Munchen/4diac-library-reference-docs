# T_FF_ILOCK_EVENT_AX


![T_FF_ILOCK_EVENT_AX_network](./T_FF_ILOCK_EVENT_AX_network.svg)

![T_FF_ILOCK_EVENT_AX](./T_FF_ILOCK_EVENT_AX.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock `T_FF_ILOCK_EVENT_AX` ist eine Subapplikation gemäß IEC 61499 und realisiert ein Toggle-Flip-Flop (T-FF) mit zusätzlicher Interlock-Funktionalität und explizitem Reset-Eingang. Er besitzt einen ereignisgesteuerten Eingang `IND` (Takt), einen Reset-Eingang `RESET` und einen Ereignis-Ausgang `SET`. Der aktuelle Zustand wird über einen unidirektionalen Adapterausgang `Q` (Typ `AX`) ausgegeben. Der Baustein dient typischerweise zur Verriegelung zweier gegenläufiger Aktoren, z. B. Ventile oder Motoren, wobei ein Set-Ereignis verwendet wird, um den Gegenpart zu deaktivieren.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
- **IND** (`Event`): Takt-/Click-Eingang. Bei jedem ansteigenden Ereignis wechselt der Ausgangszustand (Toggle).
- **RESET** (`Event`): Externer Reset-Eingang. Setzt den Ausgang zurück (Zustand „Aus").

### **Ereignis-Ausgänge**
- **SET** (`Event`): Wird ausgelöst, wenn der Ausgang auf „Ein“ geschaltet wird (Toggle von Aus auf Ein). Dieses Ereignis kann verwendet werden, um verriegelte Partnerfunktionsblöcke zurückzusetzen.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Keine.

### **Adapter**
- **Q** (Ausgang, Typ `adapter::types::unidirectional::AX`): Adapter zur Ausgabe des aktuellen Binärzustands („Ein/Aus“) als unidirektionales Signal.

## Funktionsweise

Der Baustein verhält sich wie ein Toggle-Flip-Flop mit zusätzlichem Reset und Interlock-Funktion. Intern werden drei Adapter-basierte Bausteine verwendet: `AX_E_SWITCH`, `AX_SR` und `AX_SPLIT_2`.

- **AX_E_SWITCH** agiert als Umschalter (Toggle). Bei einem `IND`-Ereignis wechselt er seinen internen Schaltzustand und aktiviert abhängig vom vorherigen Zustand entweder den Ausgang `EO0` oder `EO1`.
- **AX_SR** ist ein Set/Reset-Flip-Flop. Wird `EO0` aktiviert, erfolgt ein Set (Ausgang auf „Ein“). Wird `EO1` aktiviert oder ein `RESET` empfangen, erfolgt ein Reset (Ausgang auf „Aus“). Das `SET`-Ereignis der Subapplikation wird parallel zum Set-Signal ausgegeben.
- **AX_SPLIT_2** teilt den Ausgang des SR-Flip-Flops in zwei identische Signale: eines wird über `Q` nach außen geführt, das andere als Rückkopplung `G` an den `AX_E_SWITCH` zurückgeführt. Diese Rückkopplung speichert den aktuellen Zustand, sodass bei einem nächsten `IND`-Ereignis der Toggle in die entgegengesetzte Richtung schalten kann.

Die Interlock-Logik besteht darin, dass bei einem Übergang von „Aus“ auf „Ein“ ein `SET`-Ereignis erzeugt wird, das typischerweise an einen verriegelten Partnerbaustein gesendet wird, um dessen Ausgang zurückzusetzen. Dadurch wird verhindert, dass zwei gegeneinander wirkende Aktoren gleichzeitig aktiv sind.

## Technische Besonderheiten

- **Adapter-basierte Schnittstellen:** Der Zustand wird über einen `AX`-Adapter (unidirektional) ausgegeben, was eine einfache Verbindung mit anderen Adapter-kompatiblen Bausteinen ermöglicht.
- **Rückkopplungsschleife:** Die Rückführung des Ausgangs über `AX_SPLIT_2` an den `AX_E_SWITCH` stellt den Toggle-Mechanismus sicher, ohne dass zusätzliche Speicherbausteine in der Subapp benötigt werden.
- **Ereignisbasierter Toggle:** Der Toggle erfolgt ausschließlich über Ereignisse (`IND`), was eine schnelle, asynchrone Umschaltung in der verteilten Umgebung erlaubt.
- **Klare Interlock-Schnittstelle:** Der `SET`-Ausgang liefert ein dediziertes Ereignis, das zur Verriegelung externer Komponenten dient.

## Zustandsübersicht

Der Baustein kennt zwei Zustände:

| Zustand | Beschreibung |
|---------|--------------|
| **Aus (0)** | Ausgang `Q` = false, keine aktive Ausgabe. Nach einem `RESET` oder nach einem Toggle aus dem Zustand „Ein“ hin zu „Aus“. |
| **Ein (1)** | Ausgang `Q` = true, aktive Ausgabe. Nach einem Toggle aus „Aus“ oder nach Set (implizit durch Toggle). Das `SET`-Ereignis wird beim Übergang von „Aus“ zu „Ein“ ausgelöst. |

Der Zustandsübergang erfolgt bei jedem `IND`-Ereignis (toggle) oder bei `RESET` (setzt auf „Aus“). Während des Toggle wird der alte Zustand beibehalten und dann umgeschaltet.

## Anwendungsszenarien

- **Verriegelung von Ventilen:** Zwei Ventile, die nicht gleichzeitig geöffnet sein dürfen. Ein T-FF steuert Ventil A, der `SET`-Ausgang aktiviert den Reset eines zweiten T-FF für Ventil B, sodass nur eines offen ist.
- **Handsteuerung mit Sicherheits-Reset:** Ein Bedienimpuls (Taster) schaltet eine Last ein/aus; ein übergeordnetes Sicherheitssystem kann durch `RESET` die Anlage immer in den sicheren Zustand „Aus“ bringen.
- **Motorsteuerung mit Richtungswechsel:** Wechselnde Richtungsanweisungen (vor/zurück) werden durch Toggle realisiert, wobei die Interlock verhindert, dass beide Richtungen gleichzeitig aktiv sind.

## Vergleich mit ähnlichen Bausteinen

- **SR-Flipflop:** Ein simples SR-Flipflop benötigt separate Set- und Reset-Eingänge und besitzt kein Toggle-Verhalten. `T_FF_ILOCK_EVENT_AX` fügt den Toggle- und Interlock-Aspekt hinzu.
- **T-FF ohne Reset:** Ein einfaches Toggle-Flipflop hat keinen externen Reset und keine Interlock-Unterstützung. Dieser Baustein erweitert die Funktionalität um sichere Reset- und Verriegelungsmechanismen.
- **Standard-FB `SR`-Baustein aus IEC 61499:** Solche Bausteine sind meist nicht adapterbasiert und bieten keine automatische Rückkopplung für Toggle-Betrieb. Die Adapter-Schnittstelle von `T_FF_ILOCK_EVENT_AX` erleichtert die Einbindung in moderne Adapter-basierte Architekturen.

## Fazit

`T_FF_ILOCK_EVENT_AX` ist ein vielseitiger Baustein für verriegelte Umschalt- und Toggle-Anwendungen. Er kombiniert ein Toggle-Flip-Flop mit einem Reset-Eingang und einem speziellen Interlock-Event-Ausgang, der in Automatisierungssystemen häufig benötigt wird. Die Verwendung von Adaptern und die interne Struktur aus spezialisierten AX-Bausteinen machen ihn robust und flexibel in verteilten Steuerungsumgebungen einsetzbar. Durch die klare Trennung von Ereignis- und Signalkommunikation eignet er sich besonders für sicherheitsrelevante Steuerungen.