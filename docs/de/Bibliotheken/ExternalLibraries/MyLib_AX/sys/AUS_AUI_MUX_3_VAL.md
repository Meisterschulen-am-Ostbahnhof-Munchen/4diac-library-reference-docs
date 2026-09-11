# AUS_AUI_MUX_3_VAL


![AUS_AUI_MUX_3_VAL_network](./AUS_AUI_MUX_3_VAL_network.svg)

![AUS_AUI_MUX_3_VAL](./AUS_AUI_MUX_3_VAL.svg)

* * * * * * * * * *

## Einleitung

Der **AUS_AUI_MUX_3_VAL** ist ein Funktionsblock (SubApp), der als 3‑Wege‑Multiplexer für AUS‑Werte dient. Er ermöglicht es, über drei verschiedene Ereignis‑Eingänge einen von drei konfigurierbaren Ausgabewerten auszuwählen. Die Auswahl wird über einen AUS‑Adapter am Ausgang `OUT` zur Verfügung gestellt. Intern werden die Werte über eigene `initval_AUS`‑Bausteine initialisiert und über einen Adapter‑Multiplexer (`AUS_AUI_MUX_3`) weitergeleitet. Die Ereignissteuerung erfolgt über den Baustein `AUI_MUX_3`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI1** (Event): Event zur Auswahl von `val1`
- **EI2** (Event): Event zur Auswahl von `val2`
- **EI3** (Event): Event zur Auswahl von `val3`

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

- **val1** (USINT): Initialer Ausgabewert bei EI1
- **val2** (USINT): Initialer Ausgabewert bei EI2
- **val3** (USINT): Initialer Ausgabewert bei EI3

### **Daten-Ausgänge**

Keine direkten Datenausgänge. Der ausgewählte Wert wird über den Adapter `OUT` bereitgestellt.

### **Adapter**

- **OUT** (AUS, unidirektional): Ausgewählter AUS‑Adapter‑Output.

## Funktionsweise

Die SubApp besitzt drei Daten‑Eingänge (`val1`, `val2`, `val3`) und drei Ereignis‑Eingänge (`EI1`, `EI2`, `EI3`).  
Bei einem Ereignis an einem der drei Eingänge wird der entsprechende Wert über einen internen `initval_AUS`‑Baustein auf einen AUS‑Adapter gesetzt. Der Baustein `AUI_MUX_3` leitet das Ereignis an den Adapter‑Multiplexer `AUS_AUI_MUX_3` weiter, der dann den zugehörigen Eingang (`IN1`, `IN2` oder `IN3`) auswählt und dessen Wert auf den Ausgang `OUT` schaltet. Somit wird der an dem jeweiligen Ereignis‑Eingang eingestellte Datenwert als AUS‑Wert am Ausgang ausgegeben.

## Technische Besonderheiten

- Der Baustein ist als SubApp in der 4diac‑IDE realisiert und nutzt interne Bausteine (`initval_AUS`, `AUI_MUX_3`, `AUS_AUI_MUX_3`).
- Die Datenwerte sind als `USINT` definiert, werden aber über den AUS‑Adapter als strukturierte Werte übertragen.
- Die Lizenzierung erfolgt unter Eclipse Public License 2.0 (EPL‑2.0).

## Zustandsübersicht

Der Baustein weist keine expliziten Zustände auf. Er reagiert ereignisgesteuert: Jedes Ereignis wählt direkt den entsprechenden Wert aus. Die Auswahl wird sofort am Ausgang `OUT` aktiv.

## Anwendungsszenarien

- Umschaltung zwischen verschiedenen Betriebsmodi, die als AUS‑Werte abgebildet sind.
- Auswahl von unterschiedlichen Konfigurationswerten in Automatisierungssystemen über diskrete Ereignisse.
- Einfache Ereignis‑gesteuerte Wertauswahl in verteilten Steuerungen.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einem klassischen Multiplexer (z. B. `MUX`), der über ein Daten‑Signal auswählt, erfolgt hier die Auswahl ausschließlich über Ereignis‑Eingänge. Dadurch ist die Umschaltung ereignisbasiert und direkt an bestimmte Ereignisse gekoppelt, was für zeitkritische Steuerungen vorteilhaft sein kann. Zudem arbeitet der Baustein mit AUS‑Adaptern, die in der 4diac‑Umgebung standardisiert sind.

## Fazit

Der **AUS_AUI_MUX_3_VAL** bietet eine kompakte und klare Lösung, um zwischen drei AUS‑Werten umzuschalten. Durch die Kombination aus Ereigniseingängen und konfigurierbaren Datenwerten eignet er sich hervorragend für modulare und ereignisbasierte Automatisierungslösungen. Der Aufbau als SubApp erleichtert die Wiederverwendung und Integration in größere Systeme.
