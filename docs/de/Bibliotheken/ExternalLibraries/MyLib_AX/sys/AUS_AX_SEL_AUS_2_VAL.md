# AUS_AX_SEL_AUS_2_VAL


![AUS_AX_SEL_AUS_2_VAL_network](./AUS_AX_SEL_AUS_2_VAL_network.svg)

![AUS_AX_SEL_AUS_2_VAL](./AUS_AX_SEL_AUS_2_VAL.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AUS_AX_SEL_AUS_2_VAL** ist eine Subapplikation (SubAppType) innerhalb der Bibliothek `MyLib::sys`. Er realisiert eine binäre Auswahl zwischen zwei USINT-Werten (`val0` und `val1`) und stellt das Ergebnis über einen AUS-Adapter (`OUT`) bereit. Die Auswahl wird über einen AX-Adapter (`G`) getriggert: Ist G FALSE, wird `val0` übergeben; ist G TRUE, wird `val1` übergeben. Die Subapp dient als wiederverwendbare Komponente für Umschaltlogiken in IEC 61499-basierten Systemen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine – die Subapp besitzt keine ereignisgesteuerten Eingänge.

### **Ereignis-Ausgänge**

Keine – die Subapp besitzt keine ereignisgesteuerten Ausgänge.

### **Daten-Eingänge**

- **val0** (USINT): Ausgabewert, der bei G=FALSE (IN0) verwendet wird.
- **val1** (USINT): Ausgabewert, der bei G=TRUE (IN1) verwendet wird.

### **Daten-Ausgänge**

Keine direkten Datenausgänge; das Ergebnis wird ausschließlich über den Adapter-Ausgang `OUT` bereitgestellt.

### **Adapter**

- **G** (Socket, Typ `adapter::types::unidirectional::AX`): Binäres Auswahlsignal. G=FALSE wählt `val0`, G=TRUE wählt `val1`.
- **OUT** (Plug, Typ `adapter::types::unidirectional::AUS`): Liefert den ausgewählten Wert als AUS-Adapterausgang.

## Funktionsweise

Die Subapp nutzt zwei Instanzen des Adapters `initval_AUS` (ein Adapter vom Typ `unidirectional::AUS` mit Initialwert-Eingang), um die eingehenden USINT-Werte in AUS-Adapterwerte umzuwandeln. Diese Werte werden an einen internen Funktionsblock `F_SEL` vom Typ `adapter::iec61131::selection::AUS_AX_SEL_AUS` übergeben. Der FB `F_SEL` besitzt zwei AUS-Eingänge (`IN0`, `IN1`) und einen AX-Eingang (`G`). Gemäß dem Signal an `G` wählt er den entsprechenden Eingang aus und gibt das Ergebnis über seinen AUS-Ausgang (`OUT`) an das `OUT`-Plug der übergeordneten Subapp weiter.

Im Einzelnen:

1. `val0` und `val1` werden über die Datenverbindungen den INIT_VAL-Eingängen der jeweiligen `initval_AUS`-Adapter zugeführt.
2. Diese Adapter erzeugen stabile AUS-Werte an ihren Ausgängen.
3. Die Ausgänge werden auf `F_SEL.IN0` bzw. `F_SEL.IN1` geschaltet.
4. Das Signal `G` wird direkt an `F_SEL.G` angeschlossen.
5. Der interne FB wählt anhand von `G` den passenden Eingang aus und leitet ihn an `F_SEL.OUT` weiter.
6. Dieser Wert wird schließlich auf den Ausgangs-Adapter `OUT` der Subapp gespiegelt.

## Technische Besonderheiten

- **Adapterbasierte Schnittstellen**: Anstelle von einfachen Datentypen werden spezialisierte Adapter (`AX`, `AUS`) verwendet, die eine saubere Trennung von Steuer- und Datenfluss ermöglichen und an der Systemgrenze konfigurierbare Initialwerte bieten.
- **Keine Ereignisverarbeitung**: Die Subapp ist rein daten- und adaptergetrieben – sie benötigt keine Ereignisse, was die Einbindung in datenflussorientierte Netzwerke erleichtert.
- **Interne Konvertierung**: Die Umwandlung von `USINT` zu `AUS` erfolgt durch `initval_AUS`-Adapter, wodurch der eigentliche Auswahlbaustein `AUS_AX_SEL_AUS` unverändert wiederverwendet werden kann.
- **Lizenz** : Die Implementation ist unter der Eclipse Public License 2.0 veröffentlicht (Copyright 2026 Meisterschulen am Ostbahnhof).

## Zustandsübersicht

Die Subapp besitzt keinen internen Zustand. Sie arbeitet kombinatorisch: Die Ausgabe erfolgt unmittelbar und direkt in Abhängigkeit von den aktuellen Eingangswerten (`G`, `val0`, `val1`). Es gibt keine sequenziellen Abläufe oder Zustandsübergänge.

## Anwendungsszenarien

- **Parametervorwahl**: Umschaltung zwischen zwei Konfigurationswerten (z.B. Betriebsmodus, Sollwerte) über ein Bool-Signal.
- **Redundanzumschaltung**: Auswahl zwischen zwei alternativen Quellen (z.B. Sensoren) mit einem binären Steuersignal.
- **Test- und Diagnosefunktionen**: Umschalten zwischen Normalwert und Testwert in industriellen Steuerungen.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einem klassischen IEC 61131-3 `SEL`-Funktionsblock bietet diese Subapp eine adapterbasierte Schnittstelle (`AUS`/`AX`), die für verteilte IEC 61499-Systeme optimiert ist. Zudem ist die eigentliche Auswahlkomponente (`AUS_AX_SEL_AUS`) unabhängig von den verwendeten Datentypen und kann durch andere Adaptertypen erweitert werden. Gegenüber einer Verwendung von einfachen `USINT`-Eingängen und -Ausgängen wird hier eine höhere Abstraktionsebene erreicht, die die Kompatibilität mit komplexen Adapterbibliotheken sicherstellt.

## Fazit

Der FB **AUS_AX_SEL_AUS_2_VAL** ist eine kompakte, ereignislose Subapplikation zur binären Auswahl von Werten, die sich nahtlos in adapterbasierte IEC 61499-Netzwerke integrieren lässt. Durch die Verwendung von `initval_AUS` und `AUS_AX_SEL_AUS` wird eine robuste und flexible Lösung geschaffen, die für vielfältige Steuerungsaufgaben eingesetzt werden kann. Die einfache Konfiguration über nur zwei Daten-Eingänge und ein Binärsignal macht sie besonders gut für parametrierbare Systeme geeignet.
