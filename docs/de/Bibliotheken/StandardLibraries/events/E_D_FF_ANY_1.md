# E_D_FF_ANY_1

![E_D_FF_ANY_1](./E_D_FF_ANY_1.svg)

* * * * * * * * * *

## Einleitung

Der `E_D_FF_ANY_1` ist die Variante von [`E_D_FF_ANY`](E_D_FF_ANY.md) für **einen** zwischengespeicherten Wert beliebigen Datentyps (`ANY`) und ist Teil der Familie `E_D_FF_ANY_1` bis `E_D_FF_ANY_5` mit durchnummerierten Ports (`D1`/`Q1`). Er ist ein taktgesteuertes Daten-Latch (D-Flipflop) mit integrierter Änderungserkennung: Das Ausgangsereignis `EO` wird nur ausgelöst, wenn sich `Q1` durch den Takt ändert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **CLK**: Takt, übernimmt den aktuellen Wert von `D1`
    - **Verbundene Daten**: `D1`

### **Ereignis-Ausgänge**

- **EO**: wird ausgelöst, wenn der Takt eine Änderung von `Q1` bewirkt
    - **Verbundene Daten**: `Q1`

### **Daten-Eingänge**

- **D1** (ANY): zu speichernder Wert 1

### **Daten-Ausgänge**

- **Q1** (ANY): zwischengespeicherter Wert 1

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Bei jedem `CLK`-Ereignis wird `D1` übernommen und mit dem bisher gespeicherten Wert `Q1` verglichen. Unterscheidet sich `D1` von `Q1`, wird der Wert gespeichert und `EO` wird ausgelöst; ist der Wert unverändert, entsteht kein Ausgangsereignis. Der Baustein ist ein generischer FB (`eclipse4diac::core::GenericClassName = GEN_E_D_FF_ANY`); die native Implementierung `GEN_E_D_FF_ANY` (FORTE) wird benötigt.

## Technische Besonderheiten

- **Generischer Baustein**: Die Typen von `D1` und `Q1` sind `ANY` und werden bei der Instanziierung festgelegt
- Port-Namen `D1`/`Q1` (statt `D`/`Q` bei `E_D_FF_ANY`) entsprechen der durchnummerierten Familie
- Version 1.0 (2026-08-26): Erweiterung von `E_D_FF_ANY` auf eine nummerierte Variable

## Anwendungsszenarien

- Adapter-Wrapper um Funktionen mit einem Ergebnis, mit Ausgabe nur bei Wertänderung (z. B. `CEIL_AR`)
- Änderungsgesteuerte Weiterverarbeitung eines Wertes beliebigen Datentyps

## ⚖️ Vergleich mit ähnlichen Bausteinen

[`E_D_FF_ANY`](E_D_FF_ANY.md) speichert ebenfalls einen Wert (Ports `D`/`Q`), [`E_D_FF_ANY_2`](E_D_FF_ANY_2.md) zwei, [`E_D_FF_ANY_3`](E_D_FF_ANY_3.md) drei, [`E_D_FF_ANY_4`](E_D_FF_ANY_4.md) vier und [`E_D_FF_ANY_5`](E_D_FF_ANY_5.md) fünf Werte.

## Fazit

`E_D_FF_ANY_1` ist ein generisches, änderungserkennendes Ein-Wert-Latch, das nur dann `EO` ausgibt, wenn sich der zwischengespeicherte Wert ändert.
