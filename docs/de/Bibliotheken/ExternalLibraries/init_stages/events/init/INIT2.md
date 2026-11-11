# INIT2

![INIT2](./INIT2.svg)

![INIT2_network](./INIT2_network.svg)

* * * * * * * * * *

## Einleitung

`INIT2` ist Stufe 2 der gestuften Initialisierung (Bibliothek `init_stages`, Paket `events::init`). Der Funktionsblock ist das Gegenstück zu einem [`E_TRIG`](../../../../StandardLibraries/events/E_TRIG.md) mit dem Ereignistyp `E2Init`: Der `E_TRIG` löst den unverbundenen Eingang `INIT2` aus, `INIT2O` wird an das `INIT` des Ziels angeschlossen. Sie läuft nach dem Standard-`INIT` (`EInit`). Der Baustein enthält keine Logik.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT2 | E2Init | Initialisierungsanforderung |
| REQ | Event | Dienstanforderung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT2O | E2Init | Initialisierungsbestätigung |
| CNF | Event | Bestätigung der Anforderung |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Reine Ereignisverbindungen im inneren FBNetzwerk:

- `INIT2` → `INIT2O`
- `REQ` → `CNF`

## Verwendung

1. `INIT2` direkt neben das Ziel setzen, das in dieser Stufe initialisiert werden soll.
2. `INIT2O` mit dem `INIT` des Ziels verbinden. In Ressourcen und SubApps muss nichts durch die Hierarchie verdrahtet werden (Ausnahme: Composite-FB, siehe unten).
3. In `EMB_RES` einen `E_TRIG` mit `EVENTTYPE` `'E2Init'` in die Kette der Stufen einhängen (siehe [`init_stages`](../../index.md)).

## Technische Besonderheiten

- Composite-Funktionsblock ohne Daten und ohne internen Zustand.
- Ein `E_TRIG` mit `EVENTTYPE` `'E2Init'` löst unverbundene `INIT2`-Eingänge aus. Ein kompatibler Ereignisausgang kann einen verbundenen `INIT2`-Eingang auch direkt auslösen.
- Der `E_TRIG` muss in derselben Ressource liegen wie das Ziel. Er erreicht SubApps beliebiger Tiefe, aber nicht das Innere von Composite-FBs: Dort den `INIT2`-Eingang an der Composite-Schnittstelle herausführen.
- Die Reihenfolge der Stufen entsteht durch die Kette der `E_TRIG`: Der `E_TRIG` dieser Stufe muss an der richtigen Stelle der Kette stehen, sonst läuft die Stufe in der falschen Reihenfolge (siehe [`init_stages`](../../index.md)).
- Eine Stufe gilt als durch, wenn alle ihre Eingänge angestoßen sind, nicht, wenn die Ziele fertig initialisiert sind.
- Siehe auch [`INIT`](INIT.md) (Standardstufe, `EInit`).

## Fazit

`INIT2` verbindet Initialisierungsstufe 2 mit dem Ziel, ohne Verdrahtung durch die Hierarchie.
