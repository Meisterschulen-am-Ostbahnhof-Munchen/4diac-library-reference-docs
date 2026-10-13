# MUL_2

![MUL_2](./MUL_2.svg)

* * * * * * * * * *

## Einleitung

Der **MUL_2** ist ein generischer Funktionsbaustein, der das arithmetische Produkt zweier numerischer Eingänge berechnet (`OUT = IN1 * IN2`). Die Datentypen der Ports (`ANY_NUM`) werden bei der Instanziierung festgelegt; die native Implementierung ist `GEN_MUL`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: normale Ausführungsanforderung (mit `IN1`, `IN2`)

### **Ereignis-Ausgänge**

- **CNF**: Ausführungsbestätigung (mit `OUT`)

### **Daten-Eingänge**

- **IN1** (ANY_NUM): erster Faktor
- **IN2** (ANY_NUM): zweiter Faktor

### **Daten-Ausgänge**

- **OUT** (ANY_NUM): Produkt

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Bei `REQ` werden `IN1` und `IN2` gelesen, multipliziert und das Produkt an `OUT` zusammen mit dem Ereignis `CNF` ausgegeben.

## Technische Besonderheiten

- Generischer Baustein (Generic Class `GEN_MUL`): Die Typen der Ports sind `ANY_NUM` und werden pro Instanz festgelegt
- Zustandsloser Service-Interface-Baustein

## Anwendungsszenarien

- Skalierung eines Werts mit einem Faktor in einem Ereignis-Netzwerk
- Flächen-, Leistungs- oder Mengenberechnungen aus zwei Größen

## ⚖️ Vergleich mit ähnlichen Bausteinen

[`ADD_2`](ADD_2.md) bildet entsprechend die Summe zweier Werte; für die Multiplikation mit Adaptern gibt es die AR-basierten Varianten der Adapter-Bibliothek.

## Fazit

`MUL_2` multipliziert zwei numerische Werte beliebigen `ANY_NUM`-Typs ereignisgesteuert.
