# E_PERMIT_INVERT


![E_PERMIT_INVERT_network](./E_PERMIT_INVERT_network.svg)

![E_PERMIT_INVERT](./E_PERMIT_INVERT.svg)

* * * * * * * * * *
## Einleitung

Der Baustein **E_PERMIT_INVERT** ist eine Subapplikation nach IEC 61499-2. Er kombiniert einen booleschen Negationsbaustein mit einem Ereignis-Freigabe-Baustein und realisiert so ein **invertiertes Event-Freigabe-Gate**. Ein ankommendes Ereignis an `EI` wird nur dann an `EO` weitergegeben, wenn der Eingang `PERMIT` den Wert `FALSE` besitzt.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `EI` | Event | Event-Eingang, wird durchgelassen, wenn `PERMIT` `FALSE` ist. |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `EO` | Event | Event-Ausgang, feuert, wenn `EI` eintrifft und `PERMIT` `FALSE` ist. |

### **Daten-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `PERMIT` | BOOL | Invertierte Freigabebedingung. `FALSE` bedeutet Freigabe, `TRUE` bedeutet Sperrung. |

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Die Subapplikation besteht intern aus zwei Bausteinen:

- `F_NOT_BOOL_INIT` zur booleschen Negation
- `E_PERMIT` zur ereignisbasierten Freigabe

Der externe Eingang `PERMIT` wird an den Eingang `IN` des Negationsbausteins geführt. Dessen Ausgang `OUT` ist mit dem Freigabeeingang `PERMIT` des internen `E_PERMIT` verbunden. Ein Ereignis an `EI` wird direkt an den Ereigniseingang von `E_PERMIT` weitergeleitet.

Dadurch gilt:

- Externer `PERMIT` = `FALSE` → intern freigegeben → `EI` erzeugt `EO`
- Externer `PERMIT` = `TRUE` → intern gesperrt → `EI` erzeugt kein `EO`

Die Negation ist vollständig in der Subapplikation gekapselt. Die Umgebung sieht nur das gewünschte invertierte Freigabeverhalten.

## Technische Besonderheiten

- `E_PERMIT_INVERT` ist als `SubAppType` mit einem internen Netzwerk modelliert.
- Die Negation wird durch den IEC-61131-Baustein `F_NOT_BOOL_INIT` ausgeführt.
- Das interne `E_PERMIT` bewertet die Freigabe erst beim Eintreffen eines Ereignisses an `EI`.
- Eine Änderung von `PERMIT` allein erzeugt kein Ausgangsereignis.
- Der Baustein besitzt keine Datenausgänge und keine Adapter.
- Die Freigabelogik ist in einem einzigen wiederverwendbaren Baustein gekapselt.

## Zustandsübersicht

Da die Subapplikation keine eigene persistente Zustandsmaschine besitzt, ergibt sich die folgende äquivalente Zustandstabelle:

| Externer `PERMIT` | Interner Freigabewert | Verhalten bei `EI` |
|-------------------|------------------------|--------------------|
| `FALSE`           | `TRUE`                 | `EO` wird ausgelöst |
| `TRUE`            | `FALSE`                | `EO` wird nicht ausgelöst |

## Anwendungsszenarien

- **Aktiv-Low-Freigabe:** Ein Ereignis soll nur dann durchlaufen, wenn ein Freigabesignal `FALSE` ist.
- **Sicherheitslogik:** Ereignisse werden blockiert, solange ein Sicherheitssignal `TRUE` ist.
- **Modusumschaltung:** Eine Handsteuerung wird nur bei deaktiviertem Automatikmodus zugelassen.
- **Wiederverwendbare Kapselung:** Die invertierte Freigabelogik kann an mehreren Stellen einer Anlage identisch verwendet werden.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Verhalten |
|----------|-----------|
| `E_PERMIT` | Gibt `EI` an `EO` weiter, wenn `PERMIT` `TRUE` ist. |
| `E_PERMIT_INVERT` | Gibt `EI` an `EO` weiter, wenn `PERMIT` `FALSE` ist. |
| `F_NOT_BOOL_INIT` | Negiert einen BOOL-Wert, besitzt aber keine Event-Durchschaltung. |
| Externe Kombination aus `F_NOT_BOOL_INIT` und `E_PERMIT` | Gleiche Logik, benötigt jedoch zwei Bausteine und zusätzliche Verdrahtung. |

## Fazit

`E_PERMIT_INVERT` ist eine kompakte, klar abgegrenzte Subapplikation für invertierte Event-Freigaben. Sie kapselt die Negation und die Freigabe in einem einzigen Baustein, reduziert die externe Verdrahtung und macht die Steuerungslogik dadurch lesbarer und wartbarer. Besonders für aktiv-low-Freigaben ist dieser Baustein eine passende und wiederverwendbare Lösung.