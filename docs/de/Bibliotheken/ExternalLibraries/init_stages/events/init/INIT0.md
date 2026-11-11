# INIT0

![INIT0](./INIT0.svg)

![INIT0_network](./INIT0_network.svg)

* * * * * * * * * *

## Einleitung

`INIT0` ist Stufe 0 der gestuften Initialisierung (Bibliothek `init_stages`, Paket `events::init`). Der Funktionsblock ist das Gegenstück zu einem [`E_TRIG`](../../../../StandardLibraries/events/E_TRIG.md) mit dem Ereignistyp `E0Init`: Der `E_TRIG` löst den unverbundenen Eingang `INIT0` aus, `INIT0O` wird an das `INIT` des Ziels angeschlossen. Sie läuft vor dem Standard-`INIT` (`EInit`). Der Baustein enthält keine Logik.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT0 | E0Init | Initialisierungsanforderung |
| REQ | Event | Dienstanforderung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT0O | E0Init | Initialisierungsbestätigung |
| CNF | Event | Bestätigung der Anforderung |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Reine Ereignisverbindungen im inneren FBNetzwerk:

- `INIT0` → `INIT0O`
- `REQ` → `CNF`

## Verwendung

1. `INIT0` direkt neben das Ziel setzen, das in dieser Stufe initialisiert werden soll.
2. `INIT0O` mit dem `INIT` des Ziels verbinden. In Ressourcen und SubApps muss nichts durch die Hierarchie verdrahtet werden (Ausnahme: Composite-FB, siehe unten).
3. In `EMB_RES` einen `E_TRIG` mit `EVENTTYPE` `'E0Init'` in die Kette der Stufen einhängen (siehe [`init_stages`](../../index.md)).

## Technische Besonderheiten

- Composite-Funktionsblock ohne Daten und ohne internen Zustand.
- Ein `E_TRIG` mit `EVENTTYPE` `'E0Init'` löst unverbundene `INIT0`-Eingänge aus. Ein kompatibler Ereignisausgang kann einen verbundenen `INIT0`-Eingang auch direkt auslösen.
- Der `E_TRIG` muss in derselben Ressource liegen wie das Ziel. Er erreicht SubApps beliebiger Tiefe, aber nicht das Innere von Composite-FBs: Dort den `INIT0`-Eingang an der Composite-Schnittstelle herausführen.
- Die Reihenfolge der Stufen entsteht durch die Kette der `E_TRIG`: Der `E_TRIG` dieser Stufe muss an der richtigen Stelle der Kette stehen, sonst läuft die Stufe in der falschen Reihenfolge (siehe [`init_stages`](../../index.md)).
- Eine Stufe gilt als durch, wenn alle ihre Eingänge angestoßen sind, nicht, wenn die Ziele fertig initialisiert sind.
- Siehe auch [`INIT`](INIT.md) (Standardstufe, `EInit`).

## Fazit

`INIT0` verbindet Initialisierungsstufe 0 mit dem Ziel, ohne Verdrahtung durch die Hierarchie.
