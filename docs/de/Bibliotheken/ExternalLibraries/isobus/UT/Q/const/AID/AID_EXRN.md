# AID_EXRN

![AID_EXRN](./AID_EXRN.svg)

* * * * * * * * * *
## Einleitung

Bei `AID_EXRN` handelt es sich nicht um einen klassischen Funktionsblock, sondern um eine globale Konstantendefinition (`GlobalConstants`). Sie stellt Attribut-IDs für **External Reference Name**-Objekte im ISOBUS Universal Terminal bereit. Ziel ist es, die numerischen Kennungen für Optionen und Namen von `EXRN`-Objekten an zentraler Stelle als symbolische Konstanten verfügbar zu machen.

Die Definition ist in das Paket `isobus::UT::Q::const::AID` eingeordnet und kann über den Namen `AID_EXRN` referenziert werden.

## Schnittstellenstruktur

Da es sich um eine `GlobalConstants`-Definition handelt, besitzt `AID_EXRN` keine Ereignis- oder Datenschnittstellen im Sinne eines Funktionsblocks. Stattdessen werden globale Konstanten mit festen Werten bereitgestellt.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

Die bereitgestellten Konstanten sind:

| Konstante | Typ | Wert | Bedeutung |
|---|---|---|---|
| `OPTIONS` | `USINT` | `1` | Bitmaske für Optionen; Bit 0 = Enabled, erlaubt externe Referenz per Name |
| `NAME_0` | `USINT` | `2` | Attribut-ID für den ersten Namen des externen Referenzobjekts |
| `NAME_1` | `USINT` | `3` | Attribut-ID für den zweiten Namen des externen Referenzobjekts |

## Funktionsweise

Die globalen Konstanten von `AID_EXRN` werden verwendet, um Attribut-IDs für External-Reference-Name-Objekte zu referenzieren. Anstatt im Programmcode magische Zahlenwerte wie `1`, `2` oder `3` direkt zu verwenden, kann auf die symbolischen Konstanten zugegriffen werden, z. B. über `AID_EXRN.OPTIONS`, `AID_EXRN.NAME_0` und `AID_EXRN.NAME_1`.

Dadurch bleibt der Quellcode lesbar, eindeutig und wartungsfreundlich. Die Werte entsprechen den ISOBUS-Attribut-IDs für die jeweiligen Eigenschaften des External Reference Name Objekts.

## Technische Besonderheiten

- Die Konstanten sind vom Typ `USINT`, also 8-Bit-Ganzzahlen ohne Vorzeichen.
- Die Initialwerte sind fest als `USINT#1`, `USINT#2` und `USINT#3` deklariert.
- Die Definition ist in das Compiler-Paket `isobus::UT::Q::const::AID` eingeordnet.
- Die Werte sind zur Compilezeit festgelegt und zur Laufzeit unveränderlich.
- `OPTIONS` ist als Bitmaske definiert; Bit 0 steuert die Aktivierung der externen Referenz über einen Namen.
- Die Bereitstellung erfolgt unter der Eclipse Public License 2.0.

## Zustandsübersicht

Nicht zutreffend. `AID_EXRN` besitzt kein Zustandsverhalten und keine Zustandsmaschine. Es handelt sich um rein passive, globale Konstanten.

## Anwendungsszenarien

- ISOBUS-Implementierungen für Universal-Terminal-Objekte mit externer Namensreferenz.
- Einheitliche Referenzierung von Attribut-IDs in ST- oder FBS-Code.
- Erweiterung bestehender ISOBUS-Konstantensammlungen um standardisierte `EXRN`-Attribut-IDs.
- Vermeidung von Streuwerten und Tippfehlern durch symbolische Konstanten.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu Funktionsblöcken besitzt `AID_EXRN` keine Logik, keine Ein-/Ausgänge und keinen Zustand. Es ist vergleichbar mit anderen `GlobalConstants`-Definitionen, die Attribut-IDs für ISO-11783-6-Objekte bündeln. Der Vorteil gegenüber direkten Zahlenwerten liegt in der Lesbarkeit, Wartbarkeit und zentralen Pflege der Kennungen.

## Fazit

`AID_EXRN` ist eine kompakte globale Konstantenliste zur Unterstützung der ISOBUS-Entwicklung. Sie sorgt für klare, wiederverwendbare und leicht pflegbare Referenzen auf Attribut-IDs für External Reference Name Objekte und ergänzt die vorhandenen Konstantenpakete sinnvoll.