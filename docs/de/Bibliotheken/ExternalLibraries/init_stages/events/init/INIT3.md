# INIT3

![INIT3](./INIT3.svg)

![INIT3_network](./INIT3_network.svg)

* * * * * * * * * *

## Einleitung

`INIT3` ist Stufe 3 der gestuften Initialisierung (Bibliothek `init_stages`, Paket `events::init`). Der Funktionsblock ist das Gegenstück zu einem [`E_TRIG`](../../../../StandardLibraries/events/E_TRIG.md) mit dem Ereignistyp `E3Init`: Der `E_TRIG` löst den unverbundenen Eingang `INIT3` aus, `INIT3O` wird an das `INIT` des Ziels angeschlossen. Sie läuft nach Stufe 2. Der Baustein enthält keine Logik.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT3 | E3Init | Initialisierungsanforderung |
| REQ | Event | Dienstanforderung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT3O | E3Init | Initialisierungsbestätigung |
| CNF | Event | Bestätigung der Anforderung |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Reine Ereignisverbindungen im inneren FBNetzwerk:

- `INIT3` → `INIT3O`
- `REQ` → `CNF`

## Verwendung

1. `INIT3` direkt neben das Ziel setzen, das in dieser Stufe initialisiert werden soll.
2. `INIT3O` mit dem `INIT` des Ziels verbinden. In Ressourcen und SubApps muss nichts durch die Hierarchie verdrahtet werden (Ausnahme: Composite-FB, siehe unten).
3. In `EMB_RES` einen `E_TRIG` mit `EVENTTYPE` `'E3Init'` in die Kette der Stufen einhängen (siehe [`init_stages`](../../index.md)).

## Technische Besonderheiten

- Composite-Funktionsblock ohne Daten und ohne internen Zustand.
- Ein `E_TRIG` mit `EVENTTYPE` `'E3Init'` löst unverbundene `INIT3`-Eingänge aus. Ein kompatibler Ereignisausgang kann einen verbundenen `INIT3`-Eingang auch direkt auslösen.
- Der `E_TRIG` muss in derselben Ressource liegen wie das Ziel. Er erreicht SubApps beliebiger Tiefe, aber nicht das Innere von Composite-FBs: Dort den `INIT3`-Eingang an der Composite-Schnittstelle herausführen.
- Die Reihenfolge der Stufen entsteht durch die Kette der `E_TRIG`: Der `E_TRIG` dieser Stufe muss an der richtigen Stelle der Kette stehen, sonst läuft die Stufe in der falschen Reihenfolge (siehe [`init_stages`](../../index.md)).
- Eine Stufe gilt als durch, wenn alle ihre Eingänge angestoßen sind, nicht, wenn die Ziele fertig initialisiert sind.
- Siehe auch [`INIT`](INIT.md) (Standardstufe, `EInit`).

## Fazit

`INIT3` verbindet Initialisierungsstufe 3 mit dem Ziel, ohne Verdrahtung durch die Hierarchie.
