# E_D_FF_ANY_3

![E_D_FF_ANY_3](./E_D_FF_ANY_3.svg)

* * * * * * * * * *

## Einleitung

Der `E_D_FF_ANY_3` ist die Variante von [`E_D_FF_ANY`](E_D_FF_ANY.md) für **drei** gleichzeitig zwischengespeicherte Werte beliebigen Datentyps (`ANY`). Er ist ein taktgesteuertes Daten-Latch mit integrierter Änderungserkennung: Das Ausgangsereignis `EO` wird nur ausgelöst, wenn sich mindestens einer der drei Ausgänge `Q1`..`Q3` durch den Takt tatsächlich ändert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **CLK**: Takt, übernimmt die aktuellen Werte von `D1`, `D2` und `D3`
    - **Verbundene Daten**: `D1`, `D2`, `D3`

### **Ereignis-Ausgänge**

- **EO**: wird ausgelöst, wenn der Takt eine Änderung mindestens eines `Qi` bewirkt
    - **Verbundene Daten**: `Q1`, `Q2`, `Q3`

### **Daten-Eingänge**

- **D1** (ANY): zu speichernder Wert 1
- **D2** (ANY): zu speichernder Wert 2
- **D3** (ANY): zu speichernder Wert 3

### **Daten-Ausgänge**

- **Q1** (ANY): zwischengespeicherter Wert 1
- **Q2** (ANY): zwischengespeicherter Wert 2
- **Q3** (ANY): zwischengespeicherter Wert 3

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Bei jedem `CLK`-Ereignis werden `D1`, `D2` und `D3` übernommen und mit den bisher gespeicherten Werten `Q1`, `Q2` und `Q3` verglichen. Unterscheidet sich mindestens ein `Di` vom zugehörigen `Qi`, werden die Werte gespeichert und `EO` wird ausgelöst; sind alle Werte unverändert, entsteht kein Ausgangsereignis. Der Baustein ist ein generischer Service-Interface-FB, dessen native Implementierung `GEN_E_D_FF_ANY` (FORTE) ist.

## Technische Besonderheiten

- **Generischer Baustein**: Die Typen von `D1`..`D3` und `Q1`..`Q3` sind `ANY` und werden bei der Instanziierung festgelegt
- **Gemeinsame Änderungserkennung**: Ein einziges `EO` meldet die Änderung irgendeines der drei Werte; welcher sich geändert hat, ist am Ereignis nicht erkennbar
- Dient dazu, mehrere zusammengehörige Werte (z. B. Stellwert, Regelabweichung und Begrenzungs-Flag eines Reglers) mit nur einem Ereignis weiterzugeben und Folgeereignisse bei unveränderten Werten zu vermeiden

## Anwendungsszenarien

- Adapter-Wrapper um Funktionen, die drei Ergebnisse liefern, mit Ausgabe nur bei Wertänderung (z. B. `CTRL_PI_AR`)
- Änderungsgesteuerte Weiterverarbeitung mehrerer Werte beliebigen Datentyps

## ⚖️ Vergleich mit ähnlichen Bausteinen

`E_D_FF_ANY` speichert einen Wert, `E_D_FF_ANY_3` drei. Entsprechende Varianten mit anderer Anzahl gibt es als `E_D_FF_ANY_1`, `E_D_FF_ANY_2`, `E_D_FF_ANY_4` und `E_D_FF_ANY_5`.

## Fazit

`E_D_FF_ANY_3` ist ein generisches, änderungserkennendes Drei-Wert-Latch, das nur dann `EO` ausgibt, wenn sich mindestens einer der drei zwischengespeicherten Werte ändert.
