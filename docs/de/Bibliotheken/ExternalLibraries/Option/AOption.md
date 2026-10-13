# AOption

**Plug (Adapter-Ausgang)**

![AOption_plug](AOption_plug.svg)

**Socket (Adapter-Eingang)**

![AOption_socket](AOption_socket.svg)

* * * * * * * * * *

## Einleitung

Der Adapter `AOption` (Bibliothek `Option`) ist ein Optionsmanagement-Adapter: Er überträgt einen INIT-Request/Confirm-Handshake mit dem Event-Input-Qualifier `QI` (IEC 61499-1). Die Plug-Seite (Anforderer) kann damit eine Option bei der Socket-Seite (Optionsgruppe) anfordern/aktivieren (`QI=TRUE`) oder zurückziehen/terminieren (`QI=FALSE`); der Socket quittiert mit `INIT_CNF`.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Kommentar |
|----------|-----------|
| `INIT_CNF` | Initialisierung bestätigt (SOCKET -> PLUG). |

### **Ereignis-Ausgänge**

| Ereignis | Kommentar |
|----------|-----------|
| `INIT_REQ` | Initialisierung anfordern (PLUG -> SOCKET). Trägt die Daten `QI_OUT`. |

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|----------|-----|-----------|
| `QI_OUT` | BOOL | Event Input Qualifier zu `INIT_REQ` (IEC 61499-1): `TRUE` = Option anfordern/aktivieren (INIT_REQ+), `FALSE` = zurückziehen/terminieren (INIT_REQ-). |

(Die Angaben gelten aus Sicht der Plug-Seite; auf der Socket-Seite sind die Richtungen gespiegelt.)

## Funktionsweise

Der Plug sendet `INIT_REQ` zusammen mit `QI_OUT`. Der Socket wertet `QI_OUT` aus (Optionsgruppe aktivieren bzw. deaktivieren) und antwortet nach Abschluss mit `INIT_CNF`. Der Adapter selbst enthält keine Logik, er definiert nur die Schnittstelle. Zum Anschließen an eigene Netzwerke dienen die Wandler [`AOption_EVENT_TO_X`](./conversion/AOption_EVENT_TO_X.md) (hält den Plug) und [`AOption_X_TO_EVENT`](./conversion/AOption_X_TO_EVENT.md) (hält den Socket).

## Technische Besonderheiten

- **Handshake**: Jede Anforderung wird mit `INIT_CNF` quittiert (Request/Confirm).
- **Datenbindung**: `QI_OUT` ist mit dem Ereignis `INIT_REQ` über `With` verbunden.
- **Kein Zustandsautomat**: Der Adapter ist ein reiner Schnittstellentyp.

## Anwendungsszenarien

- Ein-/Ausschalten optionaler Funktionsgruppen (z. B. Gruppen mit E/A-Bausteinen) über einen Auswahlbaustein wie [`AOption_AD_DEMUX_2`](./selection/AOption_AD_DEMUX_2.md).

## Fazit

`AOption` bietet einen schlanken INIT_REQ/INIT_CNF-Handshake mit `QI` zum Aktivieren und Deaktivieren optionaler Teile einer Anwendung.
