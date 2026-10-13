# MUL_3

![MUL_3](./MUL_3.svg)

* * * * * * * * * *

## Einleitung

Der **MUL_3** ist ein generischer Funktionsbaustein, der das arithmetische Produkt dreier numerischer Eingänge berechnet (`OUT = IN1 * IN2 * IN3`). Die Datentypen der Ports (`ANY_NUM`) werden bei der Instanziierung festgelegt; die native Implementierung ist `GEN_MUL`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: normale Ausführungsanforderung (mit `IN1`, `IN2`, `IN3`)

### **Ereignis-Ausgänge**

- **CNF**: Ausführungsbestätigung (mit `OUT`)

### **Daten-Eingänge**

- **IN1** (ANY_NUM): erster Faktor
- **IN2** (ANY_NUM): zweiter Faktor
- **IN3** (ANY_NUM): dritter Faktor

### **Daten-Ausgänge**

- **OUT** (ANY_NUM): Produkt

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Bei `REQ` werden `IN1`, `IN2` und `IN3` gelesen, multipliziert und das Produkt an `OUT` zusammen mit dem Ereignis `CNF` ausgegeben.

## Technische Besonderheiten

- Generischer Baustein (Generic Class `GEN_MUL`): Die Typen der Ports sind `ANY_NUM` und werden pro Instanz festgelegt
- Zustandsloser Service-Interface-Baustein

## Anwendungsszenarien

- Produkt dreier Größen in einem Ereignis-Netzwerk (z. B. Volumenberechnung aus Länge, Breite, Höhe)

## ⚖️ Vergleich mit ähnlichen Bausteinen

[`MUL_2`](MUL_2.md) multipliziert zwei Werte, `MUL_3` drei.

## Fazit

`MUL_3` multipliziert drei numerische Werte beliebigen `ANY_NUM`-Typs ereignisgesteuert.
