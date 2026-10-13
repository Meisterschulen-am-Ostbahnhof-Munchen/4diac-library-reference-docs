# CEIL2

![CEIL2](./CEIL2.svg)

* * * * * * * * * *

## Einleitung

`CEIL2` ist eine Rundungsfunktion aus der OSCAT-Basic-Bibliothek: Sie liefert die kleinste ganze Zahl, die größer oder gleich `X` ist (Aufrunden). Gegenüber `CEIL` ist das Ergebnis ein `DINT`, die Funktion arbeitet also auch außerhalb des `INT`-Bereichs. Beispiele: `CEIL2(3.14) = 4`, `CEIL2(-3.14) = -3`. Portiert aus `OSCAT_basic CEIL2.TcPOU`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: Berechnungsanforderung (assoziiert mit `X`)

### **Ereignis-Ausgänge**

- **CNF**: Berechnungsbestätigung (assoziiert mit dem Funktionsergebnis)

### **Daten-Eingänge**

- **X** (REAL): zu rundender Wert

### **Daten-Ausgänge**

- **(Funktionsergebnis)** (DINT): kleinste ganze Zahl `>= X`, auf den `DINT`-Bereich begrenzt

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Die Funktion ist in Structured Text implementiert:

1. Ist `X >= 2147483647.0`, wird `2147483647` zurückgegeben.
2. Ist `X <= -2147483648.0`, wird `-2147483648` zurückgegeben.
3. Sonst wird `CEIL2 := REAL_TO_DINT(X)` berechnet; ist `DINT_TO_REAL(CEIL2) < X`, wird um 1 erhöht (`CEIL2 := CEIL2 + 1`).

## Technische Besonderheiten

- Die Bereichsprüfung schützt vor `DINT`-Überlauf: Werte außerhalb des `DINT`-Bereichs werden auf die Grenzwerte begrenzt
- Reine Funktion ohne Zustand; als Function-Baustein mit `REQ`/`CNF` in das Ereignis-Netzwerk eingebunden
- Der Ausgang besitzt keinen eigenen Namen, sondern ist der Rückgabewert der Funktion

## Anwendungsszenarien

- Aufrunden eines Messwerts oder einer berechneten Größe auf die nächste ganze Zahl, auch bei großen Beträgen

## ⚖️ Vergleich mit ähnlichen Bausteinen

[`FLOOR2`](FLOOR2.md) ist das Gegenstück zum Abrunden (größte ganze Zahl `<= X`).

## Fazit

`CEIL2` rundet einen `REAL`-Wert auf die nächste ganze Zahl auf und liefert das Ergebnis als `DINT` mit Schutz vor Überlauf.
