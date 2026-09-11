# E_PERMIT_INVERT_4


![E_PERMIT_INVERT_4_network](./E_PERMIT_INVERT_4_network.svg)

![E_PERMIT_INVERT_4](./E_PERMIT_INVERT_4.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `E_PERMIT_INVERT_4` ist eine Subapplikation mit vier Ereigniskanälen. Sie realisiert ein invertiertes Event-Freigabe-Gate: Ein Ereignis wird nur dann vom Eingang zum Ausgang durchgeschaltet, wenn das Eingangssignal `PERMIT` den Wert `FALSE` besitzt. Dazu wird `PERMIT` intern durch den Funktionsbaustein `F_NOT_BOOL_INIT` invertiert und das invertierte Signal an einen `E_PERMIT_4`-Baustein übergeben.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `EI1` | Event | Ereignis-Eingangskanal 1 |
| `EI2` | Event | Ereignis-Eingangskanal 2 |
| `EI3` | Event | Ereignis-Eingangskanal 3 |
| `EI4` | Event | Ereignis-Eingangskanal 4 |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `EO1` | Event | Ereignis-Ausgangskanal 1 |
| `EO2` | Event | Ereignis-Ausgangskanal 2 |
| `EO3` | Event | Ereignis-Ausgangskanal 3 |
| `EO4` | Event | Ereignis-Ausgangskanal 4 |

### **Daten-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `PERMIT` | BOOL | Invertierte Freigabebedingung; `TRUE` sperrt, `FALSE` gibt frei |

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Intern besteht die Subapplikation aus zwei verbundenen Bausteinen:

- `PERMIT` wird an den Eingang `IN` des Bausteins `F_NOT_BOOL_INIT` geführt.
- Der Ausgang `OUT` von `F_NOT_BOOL_INIT` ist mit dem Freigabeeingang `PERMIT` des Bausteins `E_PERMIT_4` verbunden.
- Die Ereignisse `EI1` bis `EI4` werden direkt auf die entsprechenden Ereigniseingänge von `E_PERMIT_4` geführt.
- Die Ereignisausgänge von `E_PERMIT_4` sind direkt mit `EO1` bis `EO4` verbunden.

`E_PERMIT_4` lässt ein Ereignis nur dann passieren, wenn sein `PERMIT`-Eingang den Wert `TRUE` hat. Da `F_NOT_BOOL_INIT` das externe `PERMIT`-Signal invertiert, ist die interne Freigabe nur dann `TRUE`, wenn das externe `PERMIT` den Wert `FALSE` besitzt.

Die resultierende Logik:

| Externer `PERMIT` | Interner `PERMIT` bei `E_PERMIT_4` | Ereignis `EIx` zu `EOx` |
|-------------------|------------------------------------|--------------------------|
| `TRUE`            | `FALSE`                            | gesperrt / verworfen     |
| `FALSE`           | `TRUE`                             | durchgeschaltet          |

Eine Änderung des `PERMIT`-Signals allein erzeugt keine Ereignisse. Es wird ausschließlich entschieden, ob ein ankommendes Ereignis an den jeweiligen Ausgang weitergegeben wird oder nicht.

## Technische Besonderheiten

- Die Subapplikation realisiert eine Low-aktive Freigabelogik: `FALSE` bedeutet „freigegeben“, `TRUE` bedeutet „gesperrt“.
- Sie kapselt die Invertierung des Freigabesignals und die vierkanalige Ereignisweitergabe in einem einzigen logischen Baustein.
- Es sind keine Adapter und keine Datenausgänge vorhanden.
- Die Subapplikation besitzt keine eigene Ereignisverarbeitung; sie ist rein durchschaltend und ereignisgetrieben.
- Die interne Logik reagiert nicht auf steigende oder fallende Flanken von `PERMIT`, sondern wertet den aktuellen Booleschen Wert nur dann aus, wenn ein Ereignis an `EI1` bis `EI4` eintrifft.

## Zustandsübersicht

Die Subapplikation besitzt keine explizite Zustandsmaschine. Das Verhalten kann über die Freigabebedingung beschrieben werden:

- Solange `PERMIT = TRUE` ist, werden eintreffende Ereignisse an `EI1` bis `EI4` verworfen.
- Solange `PERMIT = FALSE` ist, werden eintreffende Ereignisse an `EI1` bis `EI4` zu den entsprechenden Ausgängen `EO1` bis `EO4` durchgeschaltet.

Damit ergibt sich ein einfaches, stateless Gate-Verhalten auf Basis des aktuellen Werts von `PERMIT`.

## Anwendungsszenarien

- Einsatz als „Sperr-Gate“, wenn Ereignisse nur dann verarbeitet werden sollen, wenn ein bestimmtes Sperrsignal nicht aktiv ist.
- Verwendung in Steuerungslogiken, in denen ein Signal eine Aktion blockiert, aber nicht explizit freigibt.
- Einbindung in Sicherheits- oder Überwachungskonzepte, bei denen Ereignisse nur im nicht gesperrten Zustand weitergeleitet werden dürfen.
- Kapselung einer invertierten Freigabelogik für vier unabhängige Ereigniskanäle in einer wiederverwendbaren Subapplikation.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zum Standardbaustein `E_PERMIT_4` besitzt `E_PERMIT_INVERT_4` eine invertierte Freigabelogik. Während `E_PERMIT_4` bei `PERMIT = TRUE` durchschaltet, gibt `E_PERMIT_INVERT_4` erst bei `PERMIT = FALSE` frei.

Gegenüber einer manuellen Kombination aus einem separaten `NOT`-Baustein und einem `E_PERMIT_4` bietet die Subapplikation eine kompaktere und klar benannte Einheit. Dadurch wird die Freigabelogik an der Schnittstelle besser verständlich und die interne Verschaltung kann nicht versehentlich falsch aufgebaut werden.

## Fazit

`E_PERMIT_INVERT_4` ist ein nützlicher Baustein für Anwendungen, die eine invertierte Freigabebedingung über mehrere Ereigniskanäle benötigen. Durch die interne Kapselung von Invertierung und Freigabe wird die Applikationslogik übersichtlicher und die Wiederverwendung vereinfacht.
