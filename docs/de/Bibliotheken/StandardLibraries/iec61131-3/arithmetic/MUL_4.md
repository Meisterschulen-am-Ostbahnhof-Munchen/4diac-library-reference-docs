# MUL_4

![MUL_4](./MUL_4.svg)

* * * * * * * * * *

## Einleitung

Der **MUL_4** ist ein generischer Funktionsbaustein, der das arithmetische Produkt vierer numerischer Eingänge berechnet (`OUT = IN1 * IN2 * IN3 * IN4`). Die Datentypen der Ports (`ANY_NUM`) werden bei der Instanziierung festgelegt; die native Implementierung ist `GEN_MUL`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: normale Ausführungsanforderung (mit `IN1`, `IN2`, `IN3`, `IN4`)

### **Ereignis-Ausgänge**

- **CNF**: Ausführungsbestätigung (mit `OUT`)

### **Daten-Eingänge**

- **IN1** (ANY_NUM): erster Faktor
- **IN2** (ANY_NUM): zweiter Faktor
- **IN3** (ANY_NUM): dritter Faktor
- **IN4** (ANY_NUM): vierter Faktor

### **Daten-Ausgänge**

- **OUT** (ANY_NUM): Produkt

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

Bei `REQ` werden `IN1`, `IN2`, `IN3` und `IN4` gelesen, multipliziert und das Produkt an `OUT` zusammen mit dem Ereignis `CNF` ausgegeben.

## Technische Besonderheiten

- Generischer Baustein (Generic Class `GEN_MUL`): Die Typen der Ports sind `ANY_NUM` und werden pro Instanz festgelegt
- Zustandsloser Service-Interface-Baustein
- Version 1.0 (2026-08-28)

## Anwendungsszenarien

- Produkt vierer Größen in einem Ereignis-Netzwerk

## ⚖️ Vergleich mit ähnlichen Bausteinen

[`MUL_2`](MUL_2.md) multipliziert zwei Werte, [`MUL_3`](MUL_3.md) drei, `MUL_4` vier.

## Fazit

`MUL_4` multipliziert vier numerische Werte beliebigen `ANY_NUM`-Typs ereignisgesteuert.
