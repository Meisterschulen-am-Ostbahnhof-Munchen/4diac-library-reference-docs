# E_D_FF_ANY_2

![E_D_FF_ANY_2](E_D_FF_ANY_2.svg)

* * * * * * * * * *

## Einleitung

Der `E_D_FF_ANY_2` ist ein Daten-Latch (D-Flipflop) für **zwei `ANY`-Variablen** (generischer Baustein). Der Ausgang `EO` wird nur ausgelöst, wenn sich mindestens ein `Qi` durch den Takt ändert. Er ist die Erweiterung von `E_D_FF_ANY` auf zwei Variablen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **CLK**: Takt; übernimmt `D1` und `D2` in die Latches. Verbundene Daten: `D1`, `D2`

### **Ereignis-Ausgänge**

- **EO**: Wird ausgelöst, wenn der Takt zu einer Änderung von mindestens einem `Q` führt. Verbundene Daten: `Q1`, `Q2`

### **Daten-Eingänge**

- **D1** (`ANY`): Zu speichernder Wert 1
- **D2** (`ANY`): Zu speichernder Wert 2

### **Daten-Ausgänge**

- **Q1** (`ANY`): Gespeicherter Wert 1
- **Q2** (`ANY`): Gespeicherter Wert 2

### **Adapter**

Dieser Funktionsblock verwendet keine Adapter.

## Funktionsweise

Bei jedem `CLK`-Ereignis werden `D1` und `D2` mit den gespeicherten Werten `Q1` und `Q2` verglichen. Weicht mindestens ein `Di` vom gespeicherten `Qi` ab, werden die Werte übernommen und `EO` ausgelöst. Sind beide Werte unverändert, wird kein `EO` ausgelöst.

## Technische Besonderheiten

- Service-Interface-FB (generisch, `ANY`-Typen); er benötigt die native Implementierung `GEN_E_D_FF_ANY` (FORTE).
- Im Gegensatz zum einfachen D-Flipflop `E_D_FF` wird `EO` nur bei einer tatsächlichen Änderung ausgelöst.
- Die Variante für drei Variablen ist `E_D_FF_ANY_3`.

## Anwendungsszenarien

- Gemeinsames Latchen zusammengehörender Werte (z. B. Stellwert und Regelabweichung) und Weitergabe nur bei Änderung, um unnötige Folgeereignisse zu vermeiden

## Fazit

`E_D_FF_ANY_2` speichert zwei beliebige Werte und meldet nur dann ein Ereignis, wenn sich mindestens einer davon geändert hat.
