# AUI_AUI_MUX_7_VAL


![AUI_AUI_MUX_7_VAL_network](./AUI_AUI_MUX_7_VAL_network.svg)

![AUI_AUI_MUX_7_VAL](./AUI_AUI_MUX_7_VAL.svg)

* * * * * * * * * *

## Einleitung
Der Baustein **AUI_AUI_MUX_7_VAL** ist ein 7-Wege-Multiplexer, der AUI/UINT-Werte verarbeitet. Über sieben Ereignis-Eingänge (EI1..EI7) kann jeweils einer von sieben UINT-Werten (val1..val7) ausgewählt und als AUI-Adapterausgang bereitgestellt werden. Intern werden die eingehenden UINT-Werte in AUI-Adapter konvertiert und über eine Kombination aus Multiplexer-Bausteinen auf einen einzigen Ausgangs-Adapter geschaltet.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
- **EI1**: Event zur Auswahl von val1  
- **EI2**: Event zur Auswahl von val2  
- **EI3**: Event zur Auswahl von val3  
- **EI4**: Event zur Auswahl von val4  
- **EI5**: Event zur Auswahl von val5  
- **EI6**: Event zur Auswahl von val6  
- **EI7**: Event zur Auswahl von val7  

### **Ereignis-Ausgänge**
- Keine Ereignis-Ausgänge vorhanden.

### **Daten-Eingänge**
- **val1** (UINT): Initialer Ausgabewert bei EI1  
- **val2** (UINT): Initialer Ausgabewert bei EI2  
- **val3** (UINT): Initialer Ausgabewert bei EI3  
- **val4** (UINT): Initialer Ausgabewert bei EI4  
- **val5** (UINT): Initialer Ausgabewert bei EI5  
- **val6** (UINT): Initialer Ausgabewert bei EI6  
- **val7** (UINT): Initialer Ausgabewert bei EI7  

### **Daten-Ausgänge**
- Keine direkten Daten-Ausgänge. Die Ausgabe erfolgt über den Adapter `OUT`.

### **Adapter**
- **OUT** (Plug): Typ `adapter::types::unidirectional::AUI`  
  Ausgewählter AUI-Adapter-Output – stellt den aktuell über die Ereignisse selektierten Wert bereit.

## Funktionsweise
1. Die sieben UINT-Eingangswerte `val1`..`val7` werden jeweils an einen internen Baustein `initval_AUI_i` übergeben. Diese Bausteine konvertieren die UINT-Werte in AUI-Adapter und geben sie an ihrem Ausgang `OUT` weiter.
2. Die Ereignisse `EI1`..`EI7` werden parallel an den Baustein `AUI_MUX_7` geleitet, der basierend auf dem eingehenden Ereignis ein internes Selektionssignal (Adapter `K`) erzeugt.
3. Der Baustein `AUI_AUI_MUX_7` empfängt alle sieben vorbereiteten AUI-Adapter (`IN1`..`IN7`) sowie das Selektionssignal `K`. Er wählt denjenigen Eingang aus, dessen Index dem aktiven Ereignis entspricht, und leitet ihn an seinen Ausgang `OUT` weiter.
4. Der Ausgang `OUT` von `AUI_AUI_MUX_7` ist mit dem Adapter-Port `OUT` der Subapp verbunden und stellt so den gewünschten AUI-Wert nach außen bereit.

## Technische Besonderheiten
- **Interne Struktur**: Die Subapp besteht aus den Bausteinen `AUI_MUX_7`, `AUI_AUI_MUX_7` und sieben `initval_AUI`-Instanzen. Diese modulare Bauweise ermöglicht eine klare Trennung von Ereignissteuerung, Selektion und Wertkonvertierung.
- **Adapterbasierte Ausgabe**: Die Ausgabe erfolgt ausschließlich über den AUI-Adapter `OUT`, wodurch der Baustein nahtlos in AUI-basierte Kommunikationsketten integriert werden kann.
- **Initialwerte**: Die UINT-Werte werden als Initialwerte für die internen AUI-Adapter verwendet. Ein Ereignis setzt den Ausgang unmittelbar auf den zugehörigen Wert.

## Zustandsübersicht
Der Baustein besitzt keinen expliziten Endzustand im Sinne einer Zustandsmaschine. Er verhält sich wie ein asynchroner Multiplexer: Der aktuelle Ausgangszustand wird ausschließlich durch das zuletzt empfangene Ereignis bestimmt. Nach Aktivierung von `EIi` bleibt der Ausgang so lange auf dem Wert `vali`, bis ein anderes Ereignis ein Umschalten auslöst.

## Anwendungsszenarien
- **Parameterumschaltung**: Auswahl unterschiedlicher Konfigurationswerte (z. B. Sollwerte, Grenzwerte) durch entsprechende Ereignisse.
- **Signalrouting**: Weiterleitung von bis zu sieben AUI-Werten an eine gemeinsame Senke, wobei die Auswahl über externe Events erfolgt.
- **Test- und Diagnoseumgebungen**: Umschaltung zwischen verschiedenen Testsignalen oder simulierten Messwerten.

## Vergleich mit ähnlichen Bausteinen
Im Vergleich zu einem einfachen UINT-Multiplexer (z. B. `MUX` mit `UINT`-Datentyp) bietet dieser Baustein den Vorteil, dass die Ausgabe bereits als AUI-Adapter erfolgt. Dadurch entfällt eine nachgelagerte Konvertierung. Zudem sind die Eingangswerte als UINT definiert, was eine einfache Parametrierung aus SPS-Programmen ermöglicht. Ähnliche Bausteine ohne INIT-VAL-Unterstützung erfordern externe Initialisierung oder besitzen einen zusätzlichen Init-Eingang.

## Fazit
Der Baustein **AUI_AUI_MUX_7_VAL** ist eine kompakte und flexible Lösung zur Auswahl eines von sieben UINT-Werten mit AUI-Adapterausgang. Durch die Trennung von Ereignislogik und Adapterselektion bleibt die Funktionsweise klar und ist gut in industrielle Automatisierungskonzepte integrierbar. Die Bereitstellung von Initialwerten erleichtert die Inbetriebnahme und reduziert den externen Konfigurationsaufwand.