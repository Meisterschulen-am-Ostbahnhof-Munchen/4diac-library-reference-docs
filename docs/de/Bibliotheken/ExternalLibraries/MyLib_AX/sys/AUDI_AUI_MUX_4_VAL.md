# AUDI_AUI_MUX_4_VAL


![AUDI_AUI_MUX_4_VAL_network](./AUDI_AUI_MUX_4_VAL_network.svg)

![AUDI_AUI_MUX_4_VAL](./AUDI_AUI_MUX_4_VAL.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **AUDI_AUI_MUX_4_VAL** ist eine Subapp und realisiert einen 4-Wege-Multiplexer für AUDI-Werte. Über die Ereignis-Eingänge `EI1` bis `EI4` wird gesteuert, welcher der vier Werte `val1` bis `val4` an den AUDI-Adapter-Ausgang `OUT` durchgeschaltet wird. Die eigentliche Auswahl übernehmen interne Funktionsbausteine vom Typ `AUI_MUX_4`, `AUDI_AUI_MUX_4` und vier `initval_AUDI`-Instanzen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `EI1` | Event | Event zur Auswahl von `val1` |
| `EI2` | Event | Event zur Auswahl von `val2` |
| `EI3` | Event | Event zur Auswahl von `val3` |
| `EI4` | Event | Event zur Auswahl von `val4` |

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `val1` | UDINT | Initialer Ausgabewert bei `EI1` |
| `val2` | UDINT | Initialer Ausgabewert bei `EI2` |
| `val3` | UDINT | Initialer Ausgabewert bei `EI3` |
| `val4` | UDINT | Initialer Ausgabewert bei `EI4` |

### **Daten-Ausgänge**

Keine.

### **Adapter**

| Name | Typ | Richtung | Kommentar |
|------|-----|----------|-----------|
| `OUT` | `adapter::types::unidirectional::AUDI` | Plug / Ausgang | Ausgewählter AUDI-Adapter-Output |

## Funktionsweise

Die Subapp arbeitet als ereignisgesteuerter Multiplexer:

1. Die Daten-Eingänge `val1` bis `val4` werden jeweils an eine interne `initval_AUDI`-Instanz übergeben.
2. Diese vier Instanzen wandeln die UDINT-Werte in unidirektionale AUDI-Adapterwerte um.
3. Ein Ereignis an `EI1`, `EI2`, `EI3` oder `EI4` wird vom internen Baustein `AUI_MUX_4` verarbeitet.
4. `AUI_MUX_4` erzeugt daraus ein internes Selektionssignal `K`.
5. Der Baustein `AUDI_AUI_MUX_4` wählt anhand von `K` einen der vier AUDI-Eingänge `IN1` bis `IN4` aus.
6. Der ausgewählte AUDI-Wert wird über den Adapter-Ausgang `OUT` bereitgestellt.

Die Auswahl bleibt nach dem Auslösen eines Ereignisses aktiv, bis ein anderes Ereignis einen anderen Kanal anwählt.

## Technische Besonderheiten

- Es handelt sich um eine reine Subapp-Verdrahtung ohne eigenen ECC; das Verhalten ergibt sich aus den intern verbauten Funktionsbausteinen.
- Die vier Eingangswerte sind als `UDINT` deklariert, der Ausgang transportiert jedoch einen AUDI-Adapterwert.
- Die Umwandlung von UDINT nach AUDI übernimmt jeweils ein eigener `initval_AUDI`-Baustein.
- Die Auswahl wird über eine Kombination aus Ereignis-Multiplexer (`AUI_MUX_4`) und Adapter-Selektor (`AUDI_AUI_MUX_4`) realisiert.
- Die Subapp besitzt weder Ereignis-Ausgänge noch Daten-Ausgänge; die Ergebnisübergabe erfolgt ausschließlich über den Adapter `OUT`.

## Zustandsübersicht

| Zustand | Auslöser | Wirkung |
|---------|----------|---------|
| Kanal 1 aktiv | `EI1` | Der über `val1` initialisierte AUDI-Wert wird an `OUT` ausgegeben. |
| Kanal 2 aktiv | `EI2` | Der über `val2` initialisierte AUDI-Wert wird an `OUT` ausgegeben. |
| Kanal 3 aktiv | `EI3` | Der über `val3` initialisierte AUDI-Wert wird an `OUT` ausgegeben. |
| Kanal 4 aktiv | `EI4` | Der über `val4` initialisierte AUDI-Wert wird an `OUT` ausgegeben. |

Nach einem Ereignis bleibt der zuletzt gewählte Kanal aktiv, bis ein anderes Ereignis eintrifft.

## Anwendungsszenarien

- Umschalten zwischen vier konfigurierbaren AUDI-Werten durch einzelne Ereignisse.
- Auswahl unterschiedlicher Sollwerte oder Parameterwerte in einer Steuerungsanwendung.
- Einsatz in Test- und Simulationsumgebungen, in denen zwischen vier AUDI-basierten Datenquellen umgeschaltet werden muss.
- Wiederverwendbare Subapp in IEC-61499-Systemen, die eine strukturierte Adapter-Kommunikation verwenden.

## Vergleich mit ähnlichen Bausteinen

Der Baustein ist eine 4-fach-Variante des Prinzips eines AUDI-AUI-Multiplexers. Während eine 3-fach-Variante nur drei Eingänge und Ereignisse besitzt, erweitert `AUDI_AUI_MUX_4_VAL` dieses Konzept auf vier Kanäle. Im Gegensatz zu einem reinen Daten-Multiplexer werden die Werte nicht über einzelne Datenausgänge, sondern über einen unidirektionalen AUDI-Adapter ausgegeben. Dadurch ist der Baustein besonders für adapterbasierte Kommunikationsstrukturen geeignet.

## Fazit

`AUDI_AUI_MUX_4_VAL` ist ein kompakter und klar strukturierter 4-Wege-Multiplexer für AUDI-Werte. Er kombiniert UDINT-Eingänge, `initval_AUDI`-Konvertierung und die interne `AUDI_AUI_MUX_4`-Selektion zu einer einfachen, ereignisgesteuerten Schnittstelle. Damit eignet er sich gut für Anwendungen, die zwischen mehreren AUDI-Werten umschalten müssen.
